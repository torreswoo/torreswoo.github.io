---
bg: "posts/ruby/rails/auth/rails.png"
layout: post
title:  "Ruby On Rails | 인증 & 권한 관리"
crawlertitle: "Ruby & Rails & devise & rolify & authority"
summary: "Ruby On Rails 인증 & 권한 관리"
date:   2018-02-09 00:00:00 +0700
categories: posts
tags: ['Ruby On Rails','gem']
author: torreswoo
---

### contents
- intro : background 
- gem - devise / rolify / authority 
- devise : Flexible authentication solution for Rails with Warden 
- rolify : Role management library with resource scoping
- authority : Authority helps you authorize actions in your Rails app

---

## intro : background 
Ruby On Rails는 웹서비스를 빠르게 개발할 때 편한 웹 프레임워크이다. 웹 어드민 페이지들을 기본적을 scaffolding 해주고 웹서비스를 제공하는데 필요한 기능들을 기본적을 포함하여 제공해 주고 있다. 지금은 Rails에 대한 설명보다 인증과 권한을 위한 gem들과 그 사용방법을 설명하고자 한다.

## gem | devise / rolify / authority 
지금 알아볼 것은 사용자 로그인부터 시작해서 인증(DB, LDAP)을 거쳐서 Resource를 생성, 수정, 삭제할 수 있는 Role을 부여해서 권한을 체크하는 일련의 과정이다. 이 과정에서 필요한 gem은 devise, rolify, authority 크게 이 세 가지의 조합으로 구현할 수 있다.

| name | info  |
| --- | --- |
| [devise](https://github.com/plataformatec/devise) | Flexible authentication solution for Rails with Warden |
| [rolify](https://github.com/RolifyCommunity/rolify) | Role management library with resource scoping |
| [authority](https://github.com/nathanl/authority) | Authority helps you authorize actions in your Rails app |
| [audited](https://github.com/collectiveidea/audited) | --- |
| devise_ldap_authenticatable | --- |
| net-ldap | --- |
| omniauth | --- |


## devise | Flexible authentication solution for Rails with Warden 
devise는 User모델을 DB나 LDAP 기반의 인증을 통하는 것을 도와준다. sign in, sign up, 뿐만 아니라 password reset 기능들도 UI와 함께 기본적으로 제공된다. 비밀번호는 암호화가 되어 DB에 저장되는데 어드민이라고 확인할 수 없고, password reset을 통해 SMTP 메일서버를 설정해주면 비밀번호 초기화 기능을 제공해준다. 

```
# how to use
$ rails g devise:install
$ rails g devise User
$ rails g devise:views
```

중요한 설정은 authentication_keys인데 이 키를 가지고 내부적으로 인증(warden)을 거치고 cookie까지 세팅해서 관리하게 된다.
```ruby
# user.rb
devise :database_authenticatable, :registerable, :recoverable, :rememberable, :trackable, :validatable

# config/devise.rb
config.mailer_sender = 'please-change-me-at-config-initializers-devise@example.com'
config.authentication_keys = [:email]
```
![devise](/assets/images/posts/ruby/rails/auth/rails_devise_internel.png)

몇 가지 유용한 method를 Controller와 view에서 사용할 수 있다. 
- authenticate_user! : controller에서 인증된 사용자만 action을 수행하게 사용할 수 있다.
- current_user : 현재 로그인한 User정보를 가져올 수 있다.
- user_signed_in? : 로그인이 되었는지 확인할 수 있다.
- user_session : 사용자의 세션정보를 가져온다.

## rolify | Role management library with resource scoping
rolify는 Role을 정의하고 User에게 Role을 부여하는 것을 도와주는 gem이다. roles테이블을 생성하고  users_roles테이블을 통해 다:다 매핑을 해준다. roles에 추가적으로 resource_id, resource_type이 들어가서 resourcify로 된 모델에 대해 role정보를 가지고 있다.

```
# how to use
$ rails g rolify Role

# usage
$ user = User.find_by(username:'1001')
$ user.roles.pluck(:name, :resource_type, :resource_id)
$ user.add_role :admin
$ user.add_role :owner, Project.find(1)
```
- rolify (User에 선언)
- resourcify(Resource 모델에 선언)

![rolify](/assets/images/posts/ruby/rails/auth/rails_rolify_schema.png)


## authority | Authority helps you authorize actions in your Rails app
Controller의 Action에 따른 인증을 체크하는 것을 도와준다. 기본적인 Action 이외에 추가적으로 설정 의해서 CRUD(create, read, update, delete)이외에도 정의할 수 있다. 
***_authorizer.rb에서 creatable_by?, updatable_by?, deletable_by?, readable_by? 등을 체크할 수 있다.  
이것으로 controller의 action에서 *authorizer_action_for*를 사용해서 허가된 인증인지 체크를 할 수 있다.  
```
# how to use
$ rails g authority:install
```

```ruby
# config/authority.rb
config.controller_action_map = {
  :index   => 'read',
  :show    => 'read',
  :new     => 'create',
  :create  => 'create',
  :edit    => 'update',
  :update  => 'update',
  :destroy => 'delete',
  :permission => 'permission',
  :permission_policy => 'permission'
}


config.abilities =  {
  :create => 'creatable',
  :read   => 'readable',
  :update => 'updatable',
  :delete => 'deletable',
  :permission   => 'permissionable'
}

# projects_authorizer.rb
class ProjectAuthorizer < ApplicationAuthorizer
  class << self
    def creatable_by? user
      user.has_role_admin?
    end
  end

  def updatable_by?(user, **opts)
    owner?(user, **opts) \
      || user.has_role_admin?
  end

  def deletable_by?(user, **opts)
    owner?(user, **opts) \
      || user.has_role_admin?
  end

  def permissionable_by?(user)
    owner?(user) \
      || user.has_role_admin?
  end
end
```
![authority](/assets/images/posts/ruby/rails/auth/rails_authority.png)
  
## User Role Management UI page 
[Torres Template Project](https://github.com/torreswoo/template-Rails-Auth)
![permission_policy](/assets/images/posts/ruby/rails/auth/permission_policy.png)
![audit_log_modal](/assets/images/posts/ruby/rails/auth/audit_log_modal.png)
위 프로젝트는 설명한 gem을 이용해서 rails console을 통해 role을 부여하고 삭제한것을 UI로 컨트롤 할 수 있다
또한 Audit log를 보여주어서 각각의 리소스들의 CRUD변경이력을 누가한것인지 확인할 수 있다.


---
참고
- https://github.com/torreswoo/template-Rails-Auth  
- http://flearning-blog.tistory.com/35?category=646783
- devise : https://luciuschoi.gitbooks.io/exploring_devise/content/
- devise - warden : http://ewout.name/2010/04/http-basic-authentication-with-devise/
- Roles with Rolify : https://stackoverflow.com/questions/33950172/defining-roles-with-rolify
- FoundBlog : https://luciuschoi.gitbooks.io/foundblog/content/index.html



# Spring Security
### Spring Security 란?
* Spring에서 제공하는 **인증(Authentication)과 인가(Authorization)에 대한 처리를 위임**하는 프레임워크
* 주로 서블릿 필터(filter)와 이들로 구성된 필터체인으로 구성된 위임 모델을 사용한다.
* 보안과 관련된 체계적인 옵션을 제공해주어 개발자가 일일이 보안 관련 로직을 작성하지 않아도 된다는 장점이 있다.
### 기본 용어
1. **접근 주체**(Principal)
    * 보호된 리소스에 접근하는 대상
2. **인증**(Authentication)
    * 접근 주체에 대해 누구인지, 애플리케이션의 작업을 수행해도 되는 주체인지 확인하는 과정
    * ex) form 기반의 로그인
3. **인가**(Authorize)
    * 해당 리소스에 대해 접근 가능한 권한을 가지고 있는지 확인하는 과정
    * 인증 이후 어떤 것을 할 수 있는지 확인
4. **권한**
    * 어떠한 리소스에 대한 접근 제한
    * 모든 리소스에는 접근 제어 권한이 걸려있으며, 인가 과정에서 해당 리소스에 대해 제한된 최소한의 권한을 가졌는지 확인

### Spring Security의 주요 특징
1. **인증** (Authentication)
   * 사용자가 시스템에 로그인할 때 사용자의 아이디와 비밀번호를 확인하는 과정을 포함한다.
   * Spring Security에서는 다양한 인증 방식을 제공하며, 커스터마이징도 가능하다.
2. **권한 부여** (Authorization)
   * 인증된 사용자가 시스템의 특정 리소스 또는 기능에 접근할 수 있는지 결정하는 프로세스
   * Spring Security는 사용자의 역할(role)과 권한(permission)을 관리하고 이를 기반으로 접근 제어할 수 있다.
3. **보안 설정 관리**
   * Spring Security는 **XML** 또는 **Java Config**를 사용하여 보안 설정을 관리할 수 있다.
   * 이를 통해 보안 정책을 세부적으로 조정하고 커스터마이징할 수 있다.
4. **세션 관리 및 CSRF 방어**
   * Spring Security는 **세션 관리**를 지원하며, **CSRF(Cross-Site Request Forgery) 공격을 방어하는 기능**도 포함되어 있다.
5. **다양한 보안 기능 제공**
   * Spring Security는 로그인 폼, OAuth 인증, Remember-Me 인증 등 다양한 보안 관련 기능을 제공한다.

### Spring Security의 구조
* **SecurityContextHolder**
   * 현재 인증된 사용자의 정보를 저장하고 관리한다.
* **Authentication**
   * 사용자의 인증 정보를 나타내는 인터페이스
* **UserDetailsService**
   * 사용자 정보를 로드하는 인터페이스로, Spring Security가 인증 과정에서 사용한다.
* **UserDetails**
   * 사용자의 상세 정보를 담는 인터페이스
* **GrantedAuthority**
   * 사용자의 권한 정보를 나타내는 인터페이스
* **AuthenticationManager**
   * 인증을 처리하는 인터페이스
* **AccessDecisionManager**
   * 권한 부여 결정을 처리하는 인터페이스
* **FilterChain**
   * 여러 보안 필터를 순차적으로 적용하는 역할


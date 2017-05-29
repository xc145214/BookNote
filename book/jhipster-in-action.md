# Jhipster 实战

## 1. 安装

### 1.1 npm 安装

```powershell
λ npm -v
3.10.10

λ node -v
v6.10.3
```

### 1.2 Yeoman 安装

```powershell
λ npm install -g yo
```

### 1.3 Bower 安装

```po
npm install -g bower
```

### 1.4 Gulp 安装

```po
npm install -g gulp-cli
```

### 1.5 JHipster生成器  安装

```pow
npm install -g generator-jhipster
```

## 2. 生成项目

### 2.1 初始化项目

   ```powershell
   λ jhipster
   Running default command
   Executing jhipster:app

           ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
           ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
           ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
     ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
     ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
      ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝

                               https://jhipster.github.io

   Welcome to the JHipster Generator v4.5.1
   Documentation for creating an application: https://jhipster.github.io/creating-an-app/
   Application files will be generated in folder: E:\practise\jhispter-app
   WARNING! Failed to connect to "git://github.com"
                        1. Check your Internet connection.
                        2. If you are using an HTTP proxy, try this command: git config --global url."https://".insteadOf git://
   WARNING! yarn is not found on your computer.

   ? (1/16) Which *type* of application would you like to create? Monolithic application (recommended for simple projects)
   ? (2/16) What is the base name of your application? simpleApp
   ? (3/16) Would you like to install other generators from the JHipster Marketplace? No
   ? (4/16) What is your default Java package name? com.github.xc145214.app
   ? (5/16) Do you want to use the JHipster Registry to configure, monitor and scale your application? No
   ? (6/16) Which *type* of authentication would you like to use? JWT authentication (stateless, with a token)
   ? (7/16) Which *type* of database would you like to use? SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)
   ? (8/16) Which *production* database would you like to use? MySQL
   ? (9/16) Which *development* database would you like to use? MySQL
   ? (10/16) Do you want to use Hibernate 2nd level cache? Yes, with ehcache (local cache, for a single node)
   ? (11/16) Would you like to use Maven or Gradle for building the backend? Maven
   ? (12/16) Which other technologies would you like to use? (Press <space> to select, <a> to toggle all, <i> to inverse selection)
   ? (13/16) Which *Framework* would you like to use for the client? AngularJS 1.x
   ? (14/16) Would you like to use the LibSass stylesheet preprocessor for your CSS? No
   ? (15/16) Would you like to enable internationalization support? Yes
   ? Please choose the native language of the application? Chinese (Simplified)
   ? Please choose additional languages to install (Press <space> to select, <a> to toggle all, <i> to inverse selection)
   ? (16/16) Besides JUnit and Karma, which testing frameworks would you like to use? (Press <space> to select, <a> to toggle all, <i> to inverse sel ection)

   Installing languages: zh-cn
      create bower.json
      create package.json
      create README.md
      create .gitignore
      create .gitattributes
      create .editorconfig
      create src\main\docker\Dockerfile
      create src\main\docker\app.yml
      create src\main\docker\mysql.yml
      create src\main\docker\sonar.yml
      create mvnw
      create mvnw.cmd
      create .mvn\wrapper\maven-wrapper.jar
      create .mvn\wrapper\maven-wrapper.properties
      create pom.xml
      create src\main\resources\banner.txt
      create src\main\resources\templates\error.html
      create src\main\resources\logback-spring.xml
      create src\main\resources\config\application.yml
      create src\main\resources\config\application-dev.yml
      create src\main\resources\config\application-prod.yml
      create src\main\resources\config\liquibase\changelog\00000000000000_initial_schema.xml
      create src\main\resources\config\liquibase\master.xml
      create src\main\resources\i18n\messages.properties
      create src\main\java\com\github\xc145214\app\security\SpringSecurityAuditorAware.java
      create src\main\java\com\github\xc145214\app\security\SecurityUtils.java
      create src\main\java\com\github\xc145214\app\security\AuthoritiesConstants.java
      create src\main\java\com\github\xc145214\app\security\jwt\TokenProvider.java
      create src\main\java\com\github\xc145214\app\security\jwt\JWTConfigurer.java
      create src\main\java\com\github\xc145214\app\security\jwt\JWTFilter.java
      create src\main\java\com\github\xc145214\app\config\SecurityConfiguration.java
      create src\main\java\com\github\xc145214\app\security\DomainUserDetailsService.java
      create src\main\java\com\github\xc145214\app\security\UserNotActivatedException.java
      create src\main\java\com\github\xc145214\app\web\rest\vm\LoginVM.java
      create src\main\java\com\github\xc145214\app\web\rest\UserJWTController.java
      create src\main\java\com\github\xc145214\app\security\package-info.java
      create src\main\java\com\github\xc145214\app\SimpleApp.java
      create src\main\java\com\github\xc145214\app\ApplicationWebXml.java
      create src\main\java\com\github\xc145214\app\aop\logging\LoggingAspect.java
      create src\main\java\com\github\xc145214\app\config\DefaultProfileUtil.java
      create src\main\java\com\github\xc145214\app\config\package-info.java
      create src\main\java\com\github\xc145214\app\config\AsyncConfiguration.java
      create src\main\java\com\github\xc145214\app\config\CacheConfiguration.java
      create src\main\java\com\github\xc145214\app\config\Constants.java
      create src\main\java\com\github\xc145214\app\config\DateTimeFormatConfiguration.java
      create src\main\java\com\github\xc145214\app\config\LoggingConfiguration.java
      create src\main\java\com\github\xc145214\app\config\CloudDatabaseConfiguration.java
      create src\main\java\com\github\xc145214\app\config\DatabaseConfiguration.java
      create src\main\java\com\github\xc145214\app\config\audit\package-info.java
      create src\main\java\com\github\xc145214\app\config\audit\AuditEventConverter.java
      create src\main\java\com\github\xc145214\app\config\ApplicationProperties.java
      create src\main\java\com\github\xc145214\app\config\LocaleConfiguration.java
      create src\main\java\com\github\xc145214\app\config\LoggingAspectConfiguration.java
      create src\main\java\com\github\xc145214\app\config\MetricsConfiguration.java
      create src\main\java\com\github\xc145214\app\config\ThymeleafConfiguration.java
      create src\main\java\com\github\xc145214\app\config\WebConfigurer.java
      create src\main\java\com\github\xc145214\app\domain\package-info.java
      create src\main\java\com\github\xc145214\app\domain\AbstractAuditingEntity.java
      create src\main\java\com\github\xc145214\app\domain\PersistentAuditEvent.java
      create src\main\java\com\github\xc145214\app\repository\package-info.java
      create src\main\java\com\github\xc145214\app\service\package-info.java
      create src\main\java\com\github\xc145214\app\service\util\RandomUtil.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\ErrorConstants.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\CustomParameterizedException.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\ErrorVM.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\ExceptionTranslator.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\FieldErrorVM.java
      create src\main\java\com\github\xc145214\app\web\rest\errors\ParameterizedErrorVM.java
      create src\main\java\com\github\xc145214\app\web\rest\vm\package-info.java
      create src\main\java\com\github\xc145214\app\web\rest\vm\LoggerVM.java
      create src\main\java\com\github\xc145214\app\web\rest\util\HeaderUtil.java
      create src\main\java\com\github\xc145214\app\web\rest\util\PaginationUtil.java
      create src\main\java\com\github\xc145214\app\web\rest\package-info.java
      create src\main\java\com\github\xc145214\app\web\rest\LogsResource.java
      create src\main\java\com\github\xc145214\app\web\rest\ProfileInfoResource.java
      create src\test\java\com\github\xc145214\app\config\WebConfigurerTest.java
      create src\test\java\com\github\xc145214\app\config\WebConfigurerTestController.java
      create src\test\java\com\github\xc145214\app\web\rest\TestUtil.java
      create src\test\java\com\github\xc145214\app\web\rest\LogsResourceIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\ProfileInfoResourceIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\errors\ExceptionTranslatorIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\errors\ExceptionTranslatorTestController.java
      create src\test\resources\config\application.yml
      create src\test\resources\logback.xml
      create src\main\resources\config\liquibase\users.csv
      create src\main\resources\config\liquibase\authorities.csv
      create src\main\resources\config\liquibase\users_authorities.csv
      create src\main\resources\mails\activationEmail.html
      create src\main\resources\mails\creationEmail.html
      create src\main\resources\mails\passwordResetEmail.html
      create src\main\java\com\github\xc145214\app\domain\User.java
      create src\main\java\com\github\xc145214\app\domain\Authority.java
      create src\main\java\com\github\xc145214\app\repository\CustomAuditEventRepository.java
      create src\main\java\com\github\xc145214\app\repository\AuthorityRepository.java
      create src\main\java\com\github\xc145214\app\repository\PersistenceAuditEventRepository.java
      create src\main\java\com\github\xc145214\app\repository\UserRepository.java
      create src\main\java\com\github\xc145214\app\service\UserService.java
      create src\main\java\com\github\xc145214\app\service\MailService.java
      create src\main\java\com\github\xc145214\app\service\AuditEventService.java
      create src\main\java\com\github\xc145214\app\service\dto\package-info.java
      create src\main\java\com\github\xc145214\app\service\dto\UserDTO.java
      create src\main\java\com\github\xc145214\app\web\rest\vm\ManagedUserVM.java
      create src\main\java\com\github\xc145214\app\web\rest\UserResource.java
      create src\main\java\com\github\xc145214\app\web\rest\AccountResource.java
      create src\main\java\com\github\xc145214\app\web\rest\vm\KeyAndPasswordVM.java
      create src\main\java\com\github\xc145214\app\service\mapper\package-info.java
      create src\main\java\com\github\xc145214\app\service\mapper\UserMapper.java
      create src\main\java\com\github\xc145214\app\web\rest\AuditResource.java
      create src\test\resources\mails\testEmail.html
      create src\test\resources\i18n\messages_en.properties
      create src\test\java\com\github\xc145214\app\service\MailServiceIntTest.java
      create src\test\java\com\github\xc145214\app\service\UserServiceIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\UserResourceIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\AccountResourceIntTest.java
      create src\test\java\com\github\xc145214\app\security\SecurityUtilsUnitTest.java
      create src\test\java\com\github\xc145214\app\security\jwt\JWTFilterTest.java
      create src\test\java\com\github\xc145214\app\security\jwt\TokenProviderTest.java
      create src\test\java\com\github\xc145214\app\web\rest\UserJWTControllerIntTest.java
      create src\test\java\com\github\xc145214\app\repository\CustomAuditEventRepositoryIntTest.java
      create src\test\java\com\github\xc145214\app\web\rest\AuditResourceIntTest.java
      create gulp\handle-errors.js
      create .bowerrc
      create .eslintrc.json
      create .eslintignore
      create gulpfile.js
      create gulp\utils.js
      create gulp\serve.js
      create gulp\config.js
      create gulp\build.js
      create gulp\copy.js
      create gulp\inject.js
      create src\main\webapp\content\css\main.css
      create src\main\webapp\content\css\documentation.css
      create src\main\webapp\content\images\hipster.png
      create src\main\webapp\content\images\hipster2x.png
      create src\main\webapp\content\images\logo-jhipster.png
      create src\main\webapp\swagger-ui\index.html
      create src\main\webapp\swagger-ui\images\throbber.gif
      create src\main\webapp\favicon.ico
      create src\main\webapp\robots.txt
      create src\main\webapp\404.html
      create src\main\webapp\index.html
      create src\main\webapp\app\app.module.js
      create src\main\webapp\app\app.state.js
      create src\main\webapp\app\app.constants.js
      create src\main\webapp\app\blocks\handlers\state.handler.js
      create src\main\webapp\app\blocks\config\alert.config.js
      create src\main\webapp\app\blocks\config\http.config.js
      create src\main\webapp\app\blocks\config\localstorage.config.js
      create src\main\webapp\app\blocks\config\compile.config.js
      create src\main\webapp\app\blocks\config\uib-pager.config.js
      create src\main\webapp\app\blocks\config\uib-pagination.config.js
      create src\main\webapp\app\blocks\interceptor\auth-expired.interceptor.js
      create src\main\webapp\app\blocks\interceptor\errorhandler.interceptor.js
      create src\main\webapp\app\blocks\interceptor\notification.interceptor.js
      create src\main\webapp\app\blocks\handlers\translation.handler.js
      create src\main\webapp\app\blocks\config\translation.config.js
      create src\main\webapp\app\blocks\config\translation-storage.provider.js
      create src\main\webapp\app\blocks\interceptor\auth.interceptor.js
      create src\main\webapp\app\entities\entity.state.js
      create src\main\webapp\app\home\home.controller.js
      create src\main\webapp\app\home\home.state.js
      create src\main\webapp\app\home\home.html
      create src\main\webapp\app\layouts\navbar\navbar.controller.js
      create src\main\webapp\app\services\profiles\profile.service.js
      create src\main\webapp\app\services\profiles\page-ribbon.directive.js
      create src\main\webapp\app\layouts\navbar\navbar.html
      create src\main\webapp\app\layouts\error\error.html
      create src\main\webapp\app\layouts\error\accessdenied.html
      create src\main\webapp\app\layouts\error\error.state.js
      create src\main\webapp\app\layouts\navbar\active-menu.directive.js
      create src\main\webapp\app\account\account.state.js
      create src\main\webapp\app\account\activate\activate.controller.js
      create src\main\webapp\app\account\activate\activate.state.js
      create src\main\webapp\app\account\activate\activate.html
      create src\main\webapp\app\account\password\password.controller.js
      create src\main\webapp\app\account\password\password.state.js
      create src\main\webapp\app\account\password\password-strength-bar.directive.js
      create src\main\webapp\app\account\password\password.html
      create src\main\webapp\app\account\register\register.controller.js
      create src\main\webapp\app\account\register\register.state.js
      create src\main\webapp\app\account\register\register.html
      create src\main\webapp\app\account\reset\request\reset.request.controller.js
      create src\main\webapp\app\account\reset\request\reset.request.state.js
      create src\main\webapp\app\account\reset\request\reset.request.html
      create src\main\webapp\app\account\reset\finish\reset.finish.controller.js
      create src\main\webapp\app\account\reset\finish\reset.finish.html
      create src\main\webapp\app\account\reset\finish\reset.finish.state.js
      create src\main\webapp\app\account\settings\settings.controller.js
      create src\main\webapp\app\account\settings\settings.state.js
      create src\main\webapp\app\account\settings\settings.html
      create src\main\webapp\app\admin\admin.state.js
      create src\main\webapp\app\admin\configuration\configuration.controller.js
      create src\main\webapp\app\admin\configuration\configuration.service.js
      create src\main\webapp\app\admin\configuration\configuration.state.js
      create src\main\webapp\app\admin\configuration\configuration.html
      create src\main\webapp\app\admin\health\health.controller.js
      create src\main\webapp\app\admin\health\health.modal.controller.js
      create src\main\webapp\app\admin\health\health.service.js
      create src\main\webapp\app\admin\health\health.state.js
      create src\main\webapp\app\admin\health\health.html
      create src\main\webapp\app\admin\health\health.modal.html
      create src\main\webapp\app\admin\logs\logs.controller.js
      create src\main\webapp\app\admin\logs\logs.service.js
      create src\main\webapp\app\admin\logs\logs.state.js
      create src\main\webapp\app\admin\logs\logs.html
      create src\main\webapp\app\admin\metrics\metrics.controller.js
      create src\main\webapp\app\admin\metrics\metrics.modal.controller.js
      create src\main\webapp\app\admin\metrics\metrics.service.js
      create src\main\webapp\app\admin\metrics\metrics.state.js
      create src\main\webapp\app\admin\metrics\metrics.html
      create src\main\webapp\app\admin\metrics\metrics.modal.html
      create src\main\webapp\app\admin\docs\docs.html
      create src\main\webapp\app\admin\docs\docs.state.js
      create src\main\webapp\app\admin\audits\audits.controller.js
      create src\main\webapp\app\admin\audits\audits.service.js
      create src\main\webapp\app\admin\audits\audits.state.js
      create src\main\webapp\app\admin\audits\audits.html
      create src\main\webapp\app\admin\user-management\user-management.html
      create src\main\webapp\app\admin\user-management\user-management-detail.html
      create src\main\webapp\app\admin\user-management\user-management-dialog.html
      create src\main\webapp\app\admin\user-management\user-management-delete-dialog.html
      create src\main\webapp\app\admin\user-management\user-management.state.js
      create src\main\webapp\app\admin\user-management\user-management.controller.js
      create src\main\webapp\app\admin\user-management\user-management-detail.controller.js
      create src\main\webapp\app\admin\user-management\user-management-dialog.controller.js
      create src\main\webapp\app\admin\user-management\user-management-delete-dialog.controller.js
      create src\main\webapp\app\components\login\login.html
      create src\main\webapp\app\components\login\login.service.js
      create src\main\webapp\app\components\login\login.controller.js
      create src\main\webapp\app\components\form\show-validation.directive.js
      create src\main\webapp\app\components\form\maxbytes.directive.js
      create src\main\webapp\app\components\form\minbytes.directive.js
      create src\main\webapp\app\components\form\pagination.constants.js
      create src\main\webapp\app\components\util\base64.service.js
      create src\main\webapp\app\components\util\capitalize.filter.js
      create src\main\webapp\app\components\util\parse-links.service.js
      create src\main\webapp\app\components\util\truncate-characters.filter.js
      create src\main\webapp\app\components\util\truncate-words.filter.js
      create src\main\webapp\app\components\util\date-util.service.js
      create src\main\webapp\app\components\util\data-util.service.js
      create src\main\webapp\app\components\util\pagination-util.service.js
      create src\main\webapp\app\components\util\sort.directive.js
      create src\main\webapp\app\components\util\sort-by.directive.js
      create src\main\webapp\app\components\util\jhi-item-count.directive.js
      create src\main\webapp\app\components\alert\alert.service.js
      create src\main\webapp\app\components\alert\alert.directive.js
      create src\main\webapp\app\components\alert\alert-error.directive.js
      create src\main\webapp\app\components\language\language.filter.js
      create src\main\webapp\app\components\language\language.constants.js
      create src\main\webapp\app\components\language\language.controller.js
      create src\main\webapp\app\components\language\language.service.js
      create src\main\webapp\app\services\auth\auth.service.js
      create src\main\webapp\app\services\auth\principal.service.js
      create src\main\webapp\app\services\auth\has-authority.directive.js
      create src\main\webapp\app\services\auth\has-any-authority.directive.js
      create src\main\webapp\app\services\auth\account.service.js
      create src\main\webapp\app\services\auth\activate.service.js
      create src\main\webapp\app\services\auth\password.service.js
      create src\main\webapp\app\services\auth\password-reset-init.service.js
      create src\main\webapp\app\services\auth\password-reset-finish.service.js
      create src\main\webapp\app\services\auth\register.service.js
      create src\main\webapp\app\services\user\user.service.js
      create src\main\webapp\app\services\auth\auth.jwt.service.js
      create src\test\javascript\karma.conf.js
      create src\test\javascript\spec\helpers\module.js
      create src\test\javascript\spec\helpers\httpBackend.js
      create src\test\javascript\spec\app\admin\health\health.controller.spec.js
      create src\test\javascript\spec\app\account\password\password.controller.spec.js
      create src\test\javascript\spec\app\account\password\password-strength-bar.directive.spec.js
      create src\test\javascript\spec\app\account\settings\settings.controller.spec.js
      create src\test\javascript\spec\app\account\activate\activate.controller.spec.js
      create src\test\javascript\spec\app\account\register\register.controller.spec.js
      create src\test\javascript\spec\app\account\reset\finish\reset.finish.controller.spec.js
      create src\test\javascript\spec\app\account\reset\request\reset.request.controller.spec.js
      create src\test\javascript\spec\app\services\auth\auth.services.spec.js
      create src\test\javascript\spec\app\components\login\login.controller.spec.js
      create src\main\webapp\i18n\zh-cn\audits.json
      create src\main\webapp\i18n\zh-cn\configuration.json
      create src\main\webapp\i18n\zh-cn\error.json
      create src\main\webapp\i18n\zh-cn\gateway.json
      create src\main\webapp\i18n\zh-cn\login.json
      create src\main\webapp\i18n\zh-cn\logs.json
      create src\main\webapp\i18n\zh-cn\home.json
      create src\main\webapp\i18n\zh-cn\metrics.json
      create src\main\webapp\i18n\zh-cn\password.json
      create src\main\webapp\i18n\zh-cn\register.json
      create src\main\webapp\i18n\zh-cn\sessions.json
      create src\main\webapp\i18n\zh-cn\settings.json
      create src\main\webapp\i18n\zh-cn\user-management.json
      create src\main\webapp\i18n\zh-cn\activate.json
      create src\main\webapp\i18n\zh-cn\global.json
      create src\main\webapp\i18n\zh-cn\health.json
      create src\main\webapp\i18n\zh-cn\reset.json
      create src\main\resources\i18n\messages_zh_cn.properties


   I'm all done. Running npm install && bower install for you to install the required dependencies. If this fails, try running the command yourself.
   ```

###  2.2 安装 js依赖

```shell
npm install && bower install

gulp build
```

###   2.3 gulp build

```shell
gulp build
[16:08:54] Using gulpfile E:\practise\jhispter-app\gulpfile.js
[16:08:54] Starting 'clean'...
[16:08:54] Finished 'clean' after 24 ms
[16:08:54] Starting 'build'...
[16:08:54] Starting 'copy:i18n'...
[16:08:54] Starting 'copy:fonts'...
[16:08:54] Starting 'copy:common'...
[16:08:54] Starting 'inject:vendor'...
[16:08:54] Starting 'ngconstant:prod'...
[16:08:54] Starting 'copy:languages'...
[16:08:54] Finished 'ngconstant:prod' after 85 ms
[16:08:54] Finished 'copy:languages' after 38 ms
[16:08:54] gulp-inject 23 files into index.html.
[16:08:54] Finished 'copy:common' after 133 ms
[16:08:54] Finished 'inject:vendor' after 131 ms
[16:08:54] Finished 'copy:fonts' after 162 ms
[16:08:54] Finished 'copy:i18n' after 201 ms
[16:08:54] Starting 'copy'...
[16:08:54] Finished 'copy' after 6.53 μs
[16:08:54] Starting 'inject:app'...
[16:08:54] gulp-inject 99 files into index.html.
[16:08:54] Finished 'inject:app' after 130 ms
[16:08:54] Starting 'inject:troubleshoot'...
[16:08:54] gulp-inject 1 files into index.html.
[16:08:54] Finished 'inject:troubleshoot' after 4.47 ms
[16:08:54] Starting 'images'...
[16:08:54] Starting 'styles'...
[16:08:54] Starting 'html'...
[16:08:54] Starting 'copy:swagger'...
[16:08:54] Starting 'copy:images'...
[16:08:54] Finished 'copy:images' after 11 ms
[16:08:54] Finished 'styles' after 29 ms
[16:08:54] gulp-notify: [JHipster Gulp Build] Error: ϵͳ�Ҳ���ָ����·����

[16:08:54] Finished 'images' after 160 ms
[16:08:54] gulp-notify: [JHipster Gulp Build] Error: ϵͳ�Ҳ���ָ����·����

[16:08:54] gulp-notify: [JHipster Gulp Build] Error: ϵͳ�Ҳ���ָ����·����

[16:08:54] gulp-imagemin: Minified 0 images
[16:08:55] Finished 'html' after 279 ms
[16:08:55] Finished 'copy:swagger' after 378 ms
[16:08:55] Starting 'assets:prod'...
[16:09:08] Finished 'assets:prod' after 14 s
[16:09:08] Finished 'build' after 15 s

```

### 2.4 启动运行

```shell
16:10:34.435 [main] DEBUG org.springframework.boot.devtools.settings.DevToolsSettings - Included patterns for restart : []
16:10:34.438 [main] DEBUG org.springframework.boot.devtools.settings.DevToolsSettings - Excluded patterns for restart : [/spring-boot-starter/target/classes/, /spring-boot-autoconfigure/target/classes/, /spring-boot-starter-[\w-]+/, /spring-boot/target/classes/, /spring-boot-actuator/target/classes/, /spring-boot-devtools/target/classes/]
16:10:34.439 [main] DEBUG org.springframework.boot.devtools.restart.ChangeableUrls - Matching URLs for reloading : [file:/E:/practise/jhispter-app/target/classes/]

        ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
        ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
        ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
  ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
  ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
   ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝

:: JHipster 🤓  :: Running Spring Boot 1.5.3.RELEASE ::
:: http://jhipster.github.io ::

2017-05-27 16:10:36.152  INFO 10016 --- [  restartedMain] com.github.xc145214.app.SimpleApp        : Starting SimpleApp on HL-JSB-XIACHUAN with PID 10016 (E:\practise\jhispter-app\target\classes started by xiachuan in E:\practise\jhispter-app)
2017-05-27 16:10:36.176 DEBUG 10016 --- [  restartedMain] com.github.xc145214.app.SimpleApp        : Running with Spring Boot v1.5.3.RELEASE, Spring v4.3.8.RELEASE
2017-05-27 16:10:36.176  INFO 10016 --- [  restartedMain] com.github.xc145214.app.SimpleApp        : The following profiles are active: swagger,dev
2017-05-27 16:10:37.089 DEBUG 10016 --- [kground-preinit] org.jboss.logging                        : Logging Provider: org.jboss.logging.Slf4jLoggerProvider found via system property
2017-05-27 16:10:40.899 DEBUG 10016 --- [  restartedMain] c.g.x.app.config.AsyncConfiguration      : Creating Async Task Executor
2017-05-27 16:10:42.115 DEBUG 10016 --- [  restartedMain] c.e.c.E.github.xc145214.app.domain.User  : Initialize successful.
2017-05-27 16:10:42.128 DEBUG 10016 --- [  restartedMain] c.e.c.E.g.xc145214.app.domain.Authority  : Initialize successful.
2017-05-27 16:10:42.132 DEBUG 10016 --- [  restartedMain] c.e.c.E.g.x.app.domain.User.authorities  : Initialize successful.
2017-05-27 16:10:42.476 DEBUG 10016 --- [  restartedMain] c.g.x.app.config.MetricsConfiguration    : Registering JVM gauges
2017-05-27 16:10:42.490 DEBUG 10016 --- [  restartedMain] c.g.x.app.config.MetricsConfiguration    : Monitoring the datasource
2017-05-27 16:10:42.490 DEBUG 10016 --- [  restartedMain] c.g.x.app.config.MetricsConfiguration    : Initializing Metrics JMX reporting
2017-05-27 16:10:43.567 DEBUG 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Registering CORS filter
2017-05-27 16:10:43.724  INFO 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Web application configuration, using profiles: swagger
2017-05-27 16:10:43.724 DEBUG 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Initializing Metrics registries
2017-05-27 16:10:43.727 DEBUG 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Registering Metrics Filter
2017-05-27 16:10:43.728 DEBUG 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Registering Metrics Servlet
2017-05-27 16:10:43.732  INFO 10016 --- [  restartedMain] c.g.xc145214.app.config.WebConfigurer    : Web application fully configured
2017-05-27 16:10:44.153 DEBUG 10016 --- [  restartedMain] c.g.x.app.config.DatabaseConfiguration   : Configuring Liquibase
2017-05-27 16:10:44.255  WARN 10016 --- [-app-Executor-1] i.g.j.c.liquibase.AsyncSpringLiquibase   : Starting Liquibase asynchronously, your database might not be ready at startup!
2017-05-27 16:10:50.269 DEBUG 10016 --- [-app-Executor-1] i.g.j.c.liquibase.AsyncSpringLiquibase   : Liquibase has updated your database in 6013 ms
2017-05-27 16:10:50.270  WARN 10016 --- [-app-Executor-1] i.g.j.c.liquibase.AsyncSpringLiquibase   : Warning, Liquibase took more than 5 seconds to start up!
2017-05-27 16:10:56.822 DEBUG 10016 --- [  restartedMain] i.g.j.c.apidoc.SwaggerConfiguration      : Starting Swagger
2017-05-27 16:10:56.829 DEBUG 10016 --- [  restartedMain] i.g.j.c.apidoc.SwaggerConfiguration      : Started Swagger in 7 ms
2017-05-27 16:10:58.262  INFO 10016 --- [  restartedMain] com.github.xc145214.app.SimpleApp        : Started SimpleApp in 23.771 seconds (JVM running for 25.369)
2017-05-27 16:10:58.263  INFO 10016 --- [  restartedMain] com.github.xc145214.app.SimpleApp        : 
----------------------------------------------------------
	Application 'simpleApp' is running! Access URLs:
	Local: 		http://localhost:8080
	External: 	http://172.16.1.145:8080
	Profile(s): 	[swagger, dev]
----------------------------------------------------------
```

## 3. 扩展项目

基于JHipster生成的app只是对user的CRUD，在实际项目中肯定有各种业务逻辑

所以扩展原有项目是势在必行的，扩展时以下是可能用到的技术

-        [JDL 数据模型、在线工具](https://jhipster.github.io/jdl-studio/)
-        [UML 建模](https://jhipster.github.io/jhipster-uml/)
-        [创建一个Entity](https://jhipster.github.io/creating-an-entity/)
-        [服务端开发](https://jhipster.github.io/development/)
-        [客户端开发](https://jhipster.github.io/using-angularjs/)
-        [Web Sockets](https://jhipster.github.io/using-websockets/)
-        [多环节管理](https://jhipster.github.io/profiles/)
-        [UI 开发 自定义bootstrap](https://jhipster.github.io/customizing-bootstrap/)

### 3.1 创建一个实体类 Entity

JHipster支持在线进行表结构设计[工具JDL](https://jhipster.github.io/creating-an-entity/)，官网有对 Entity fields、Field types、Validation、Entity relationships、Data Transfer Objects (DTOs)、Pagination、Updating an existing entity 进行讲解；

If you used the JDL Studio:

- You can generate entities from a JDL file using the `import-jdl` sub-generator, by running `yo jhipster:import-jdl your-jdl-file.jh`.
- If you want to use JHipster UML instead of the `import-jdl` sub-generator, you need to install it by running `npm install -g jhipster-uml`, and then run `jhipster-uml yourFileName.jh`.

#### 3.1.1 通过文件创建

当在项目中执行 【yo jhipster:import-jdl your-jdl-file.jh】后，JHipster会自动生成服务端、客户端、表结构xml文件，create后的文件如下:

```shell
E:\practise\jhispter-app>yo jhipster:import-jdl entity/entity.jh
The jdl is being parsed.
Writing entity JSON files.
Generating entities.

Found the .jhipster/Region.json configuration file, entity can be automatically generated!


The entity Region is being updated.


Found the .jhipster/Country.json configuration file, entity can be automatically generated!


The entity Country is being updated.


Found the .jhipster/Location.json configuration file, entity can be automatically generated!


The entity Location is being updated.


Found the .jhipster/Department.json configuration file, entity can be automatically generated!


The entity Department is being updated.


Found the .jhipster/Task.json configuration file, entity can be automatically generated!


The entity Task is being updated.


Found the .jhipster/Employee.json configuration file, entity can be automatically generated!


The entity Employee is being updated.


Found the .jhipster/Job.json configuration file, entity can be automatically generated!


The entity Job is being updated.


Found the .jhipster/JobHistory.json configuration file, entity can be automatically generated!


The entity JobHistory is being updated.

   create src\main\resources\config\liquibase\changelog\20170527083830_added_entity_Region.xml
   create src\main\java\com\github\xc145214\app\domain\Region.java
   create src\main\java\com\github\xc145214\app\repository\RegionRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\RegionResource.java
   create src\main\java\com\github\xc145214\app\service\RegionService.java
   create src\main\java\com\github\xc145214\app\service\impl\RegionServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\RegionDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\EntityMapper.java
   create src\main\java\com\github\xc145214\app\service\mapper\RegionMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\RegionResourceIntTest.java
 conflict src\main\resources\config\liquibase\master.xml
? Overwrite src\main\resources\config\liquibase\master.xml? overwrite
    force src\main\resources\config\liquibase\master.xml
 conflict src\main\java\com\github\xc145214\app\config\CacheConfiguration.java
? Overwrite src\main\java\com\github\xc145214\app\config\CacheConfiguration.java? overwrite
    force src\main\java\com\github\xc145214\app\config\CacheConfiguration.java
   create src\main\webapp\app\entities\region\regionsmySuffix.html
   create src\main\webapp\app\entities\region\region-my-suffix-detail.html
   create src\main\webapp\app\entities\region\region-my-suffix-dialog.html
   create src\main\webapp\app\entities\region\region-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\region\region-my-suffix.state.js
   create src\main\webapp\app\entities\region\region-my-suffix.controller.js
   create src\main\webapp\app\entities\region\region-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\region\region-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\region\region-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\region\region-my-suffix.service.js
   create src\test\javascript\spec\app\entities\region\region-my-suffix-detail.controller.spec.js
 conflict src\main\webapp\app\layouts\navbar\navbar.html
? Overwrite src\main\webapp\app\layouts\navbar\navbar.html? overwrite
    force src\main\webapp\app\layouts\navbar\navbar.html
   create src\main\webapp\i18n\zh-cn\region.json
 conflict src\main\webapp\i18n\zh-cn\global.json
? Overwrite src\main\webapp\i18n\zh-cn\global.json? overwrite
    force src\main\webapp\i18n\zh-cn\global.json
   create src\main\resources\config\liquibase\changelog\20170527083831_added_entity_Country.xml
   create src\main\resources\config\liquibase\changelog\20170527083831_added_entity_constraints_Country.xml
   create src\main\java\com\github\xc145214\app\domain\Country.java
   create src\main\java\com\github\xc145214\app\repository\CountryRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\CountryResource.java
   create src\main\java\com\github\xc145214\app\service\CountryService.java
   create src\main\java\com\github\xc145214\app\service\impl\CountryServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\CountryDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\CountryMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\CountryResourceIntTest.java
   create src\main\webapp\app\entities\country\countriesmySuffix.html
   create src\main\webapp\app\entities\country\country-my-suffix-detail.html
   create src\main\webapp\app\entities\country\country-my-suffix-dialog.html
   create src\main\webapp\app\entities\country\country-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\country\country-my-suffix.state.js
   create src\main\webapp\app\entities\country\country-my-suffix.controller.js
   create src\main\webapp\app\entities\country\country-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\country\country-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\country\country-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\country\country-my-suffix.service.js
   create src\test\javascript\spec\app\entities\country\country-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\country.json
   create src\main\resources\config\liquibase\changelog\20170527083832_added_entity_Location.xml
   create src\main\resources\config\liquibase\changelog\20170527083832_added_entity_constraints_Location.xml
   create src\main\java\com\github\xc145214\app\domain\Location.java
   create src\main\java\com\github\xc145214\app\repository\LocationRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\LocationResource.java
   create src\main\java\com\github\xc145214\app\service\LocationService.java
   create src\main\java\com\github\xc145214\app\service\impl\LocationServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\LocationDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\LocationMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\LocationResourceIntTest.java
   create src\main\webapp\app\entities\location\locationsmySuffix.html
   create src\main\webapp\app\entities\location\location-my-suffix-detail.html
   create src\main\webapp\app\entities\location\location-my-suffix-dialog.html
   create src\main\webapp\app\entities\location\location-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\location\location-my-suffix.state.js
   create src\main\webapp\app\entities\location\location-my-suffix.controller.js
   create src\main\webapp\app\entities\location\location-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\location\location-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\location\location-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\location\location-my-suffix.service.js
   create src\test\javascript\spec\app\entities\location\location-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\location.json
   create src\main\resources\config\liquibase\changelog\20170527083833_added_entity_Department.xml
   create src\main\resources\config\liquibase\changelog\20170527083833_added_entity_constraints_Department.xml
   create src\main\java\com\github\xc145214\app\domain\Department.java
   create src\main\java\com\github\xc145214\app\repository\DepartmentRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\DepartmentResource.java
   create src\main\java\com\github\xc145214\app\service\DepartmentService.java
   create src\main\java\com\github\xc145214\app\service\impl\DepartmentServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\DepartmentDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\DepartmentMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\DepartmentResourceIntTest.java
   create src\main\webapp\app\entities\department\departmentsmySuffix.html
   create src\main\webapp\app\entities\department\department-my-suffix-detail.html
   create src\main\webapp\app\entities\department\department-my-suffix-dialog.html
   create src\main\webapp\app\entities\department\department-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\department\department-my-suffix.state.js
   create src\main\webapp\app\entities\department\department-my-suffix.controller.js
   create src\main\webapp\app\entities\department\department-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\department\department-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\department\department-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\department\department-my-suffix.service.js
   create src\test\javascript\spec\app\entities\department\department-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\department.json
   create src\main\resources\config\liquibase\changelog\20170527083834_added_entity_Task.xml
   create src\main\java\com\github\xc145214\app\domain\Task.java
   create src\main\java\com\github\xc145214\app\repository\TaskRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\TaskResource.java
   create src\main\java\com\github\xc145214\app\service\TaskService.java
   create src\main\java\com\github\xc145214\app\service\impl\TaskServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\TaskDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\TaskMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\TaskResourceIntTest.java
   create src\main\webapp\app\entities\task\tasksmySuffix.html
   create src\main\webapp\app\entities\task\task-my-suffix-detail.html
   create src\main\webapp\app\entities\task\task-my-suffix-dialog.html
   create src\main\webapp\app\entities\task\task-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\task\task-my-suffix.state.js
   create src\main\webapp\app\entities\task\task-my-suffix.controller.js
   create src\main\webapp\app\entities\task\task-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\task\task-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\task\task-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\task\task-my-suffix.service.js
   create src\test\javascript\spec\app\entities\task\task-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\task.json
   create src\main\resources\config\liquibase\changelog\20170527083835_added_entity_Employee.xml
   create src\main\resources\config\liquibase\changelog\20170527083835_added_entity_constraints_Employee.xml
   create src\main\java\com\github\xc145214\app\domain\Employee.java
   create src\main\java\com\github\xc145214\app\repository\EmployeeRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\EmployeeResource.java
   create src\main\java\com\github\xc145214\app\service\dto\EmployeeDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\EmployeeMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\EmployeeResourceIntTest.java
   create src\main\webapp\app\entities\employee\employeesmySuffix.html
   create src\main\webapp\app\entities\employee\employee-my-suffix-detail.html
   create src\main\webapp\app\entities\employee\employee-my-suffix-dialog.html
   create src\main\webapp\app\entities\employee\employee-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\employee\employee-my-suffix.state.js
   create src\main\webapp\app\entities\employee\employee-my-suffix.controller.js
   create src\main\webapp\app\entities\employee\employee-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\employee\employee-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\employee\employee-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\employee\employee-my-suffix.service.js
   create src\test\javascript\spec\app\entities\employee\employee-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\employee.json
   create src\main\resources\config\liquibase\changelog\20170527083836_added_entity_Job.xml
   create src\main\resources\config\liquibase\changelog\20170527083836_added_entity_constraints_Job.xml
   create src\main\java\com\github\xc145214\app\domain\Job.java
   create src\main\java\com\github\xc145214\app\repository\JobRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\JobResource.java
   create src\main\java\com\github\xc145214\app\service\dto\JobDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\JobMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\JobResourceIntTest.java
   create src\main\webapp\app\entities\job\jobsmySuffix.html
   create src\main\webapp\app\entities\job\job-my-suffix-detail.html
   create src\main\webapp\app\entities\job\job-my-suffix-dialog.html
   create src\main\webapp\app\entities\job\job-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\job\job-my-suffix.state.js
   create src\main\webapp\app\entities\job\job-my-suffix.controller.js
   create src\main\webapp\app\entities\job\job-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\job\job-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\job\job-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\job\job-my-suffix.service.js
   create src\test\javascript\spec\app\entities\job\job-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\job.json
   create src\main\resources\config\liquibase\changelog\20170527083837_added_entity_JobHistory.xml
   create src\main\resources\config\liquibase\changelog\20170527083837_added_entity_constraints_JobHistory.xml
   create src\main\java\com\github\xc145214\app\domain\JobHistory.java
   create src\main\java\com\github\xc145214\app\repository\JobHistoryRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\JobHistoryResource.java
   create src\main\java\com\github\xc145214\app\service\JobHistoryService.java
   create src\main\java\com\github\xc145214\app\service\impl\JobHistoryServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\JobHistoryDTO.java
   create src\main\java\com\github\xc145214\app\service\mapper\JobHistoryMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\JobHistoryResourceIntTest.java
   create src\main\java\com\github\xc145214\app\domain\enumeration\Language.java
   create src\main\webapp\i18n\zh-cn\language.json
   create src\main\webapp\app\entities\job-history\job-historiesmySuffix.html
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-detail.html
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-dialog.html
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-delete-dialog.html
   create src\main\webapp\app\entities\job-history\job-history-my-suffix.state.js
   create src\main\webapp\app\entities\job-history\job-history-my-suffix.controller.js
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-dialog.controller.js
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-delete-dialog.controller.js
   create src\main\webapp\app\entities\job-history\job-history-my-suffix-detail.controller.js
   create src\main\webapp\app\entities\job-history\job-history-my-suffix.service.js
   create src\test\javascript\spec\app\entities\job-history\job-history-my-suffix-detail.controller.spec.js
   create src\main\webapp\i18n\zh-cn\jobHistory.json

Running gulp Inject to add javascript to index

[16:38:57] Using gulpfile E:\practise\jhispter-app\gulpfile.js
[16:38:57] Starting 'inject:app'...
[16:38:57] gulp-inject 147 files into index.html.
[16:38:57] Finished 'inject:app' after 204 ms


```

3.1.2 通过命令创建

```shell
jhipster entity author
Executing jhipster:entity author

The entity author is being created.


Generating field #1

? Do you want to add a field to your entity? Yes
? What is the name of your field? name
? What is the type of your field? (Use arrow keys)
> String
  Integer
? What is the type of your field? String
? Do you want to add validation rules to your field? Yes
? Which validation rules do you want to add? Required

================= Author =================
Fields
name (String) required 


Generating field #2

? Do you want to add a field to your entity? Yes
? What is the name of your field? birthDate
? What is the type of your field? LocalDate
? Do you want to add validation rules to your field? No

================= Author =================
Fields
name (String) required 
birthDate (LocalDate)


Generating field #3

? Do you want to add a field to your entity? No

================= Author =================
Fields
name (String) required 
birthDate (LocalDate)


Generating relationships to other entities

? Do you want to add a relationship to another entity? No

================= Author =================
Fields
name (String) required 
birthDate (LocalDate)



? Do you want to use a Data Transfer Object (DTO)? [BETA] Yes, generate a DTO with MapStruct
? Do you want to use separate service class for your business logic? Yes, generate a separate service interface and implementation
? Do you want pagination on your entity? Yes, with pagination links

Everything is configured, generating the entity...

   create .jhipster\Author.json
   create src\main\resources\config\liquibase\changelog\20170527090145_added_entity_Author.xml
   create src\main\java\com\github\xc145214\app\domain\Author.java
   create src\main\java\com\github\xc145214\app\repository\AuthorRepository.java
   create src\main\java\com\github\xc145214\app\web\rest\AuthorResource.java
   create src\main\java\com\github\xc145214\app\service\AuthorService.java
   create src\main\java\com\github\xc145214\app\service\impl\AuthorServiceImpl.java
   create src\main\java\com\github\xc145214\app\service\dto\AuthorDTO.java
identical src\main\java\com\github\xc145214\app\service\mapper\EntityMapper.java
   create src\main\java\com\github\xc145214\app\service\mapper\AuthorMapper.java
   create src\test\java\com\github\xc145214\app\web\rest\AuthorResourceIntTest.java
 conflict src\main\resources\config\liquibase\master.xml
? Overwrite src\main\resources\config\liquibase\master.xml? overwrite
    force src\main\resources\config\liquibase\master.xml
 conflict src\main\java\com\github\xc145214\app\config\CacheConfiguration.java
? Overwrite src\main\java\com\github\xc145214\app\config\CacheConfiguration.java? overwrite
    force src\main\java\com\github\xc145214\app\config\CacheConfiguration.java
   create src\main\webapp\app\entities\author\authors.html
   create src\main\webapp\app\entities\author\author-detail.html
   create src\main\webapp\app\entities\author\author-dialog.html
   create src\main\webapp\app\entities\author\author-delete-dialog.html
   create src\main\webapp\app\entities\author\author.state.js
   create src\main\webapp\app\entities\author\author.controller.js
   create src\main\webapp\app\entities\author\author-dialog.controller.js
   create src\main\webapp\app\entities\author\author-delete-dialog.controller.js
   create src\main\webapp\app\entities\author\author-detail.controller.js
   create src\main\webapp\app\entities\author\author.service.js
   create src\test\javascript\spec\app\entities\author\author-detail.controller.spec.js
 conflict src\main\webapp\app\layouts\navbar\navbar.html
? Overwrite src\main\webapp\app\layouts\navbar\navbar.html? overwrite
    force src\main\webapp\app\layouts\navbar\navbar.html
   create src\main\webapp\i18n\zh-cn\author.json
 conflict src\main\webapp\i18n\zh-cn\global.json
? Overwrite src\main\webapp\i18n\zh-cn\global.json? overwrite
    force src\main\webapp\i18n\zh-cn\global.json

Running `gulp inject` to add JavaScript to index.html

[17:01:58] Using gulpfile E:\practise\jhispter-app\gulpfile.js
[17:01:58] Starting 'inject:app'...
[17:01:59] gulp-inject 153 files into index.html.
[17:01:59] Finished 'inject:app' after 205 ms
Execution complete

```


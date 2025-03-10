Configuration
=============

The following configuration values are used by Flask-Security:

Core
--------------

These configuration keys are used globally across all features.

.. py:data:: SECRET_KEY

    This is actually part of Flask - but is used by Flask-Security to sign all tokens.
    It is critical this is set to a strong value. For python3 consider using: ``secrets.token_urlsafe()``

.. py:data:: SECURITY_BLUEPRINT_NAME

    Specifies the name for the Flask-Security blueprint.

    Default: ``security``.

.. py:data:: SECURITY_URL_PREFIX

    Specifies the URL prefix for the Flask-Security blueprint.

    Default: ``None``.

.. py:data:: SECURITY_SUBDOMAIN

    Specifies the subdomain for the Flask-Security blueprint. If your authenticated
    content is on a different subdomain, also enable :py:data:`SECURITY_REDIRECT_ALLOW_SUBDOMAINS`.

    Default: ``None``.
.. py:data:: SECURITY_FLASH_MESSAGES

    Specifies whether or not to flash messages during security procedures.

    Default: ``True``.
.. py:data:: SECURITY_I18N_DOMAIN

    Specifies the name for domain used for translations.

    Default: ``flask_security``.
.. py:data:: SECURITY_I18N_DIRNAME

    Specifies the directory containing the ``MO`` files used for translations.

    Default: ``[PATH_LIB]/flask_security/translations``.

.. py:data:: SECURITY_PASSWORD_HASH

    Specifies the password hash algorithm to use when hashing passwords.
    Recommended values for production systems are ``bcrypt``, ``argon2``, ``sha512_crypt``, or
    ``pbkdf2_sha512``. Some algorithms require the installation  of a backend package (e.g. `bcrypt`_, `argon2`_).

    Default:``bcrypt``.

.. py:data:: SECURITY_PASSWORD_SCHEMES

    List of support password hash algorithms. ``SECURITY_PASSWORD_HASH``
    must be from this list. Passwords encrypted with any of these schemes will be honored.

.. py:data:: SECURITY_DEPRECATED_PASSWORD_SCHEMES

    List of password hash algorithms that are considered weak and
    will be accepted, however on first use, will be re-hashed to the current
    setting of ``SECURITY_PASSWORD_HASH``.

    Default: ``["auto"]`` which means any password found that wasn't
    hashed using ``SECURITY_PASSWORD_HASH`` will be re-hashed.

.. py:data:: SECURITY_PASSWORD_SALT

    Specifies the HMAC salt. This is required for all schemes that
    are configured for double hashing. A good salt can be generated using:
    ``secrets.SystemRandom().getrandbits(128)``.

    Default: ``None``.

.. py:data:: SECURITY_PASSWORD_SINGLE_HASH

    A list of schemes that should not be hashed twice. By default, passwords are
    hashed twice, first with ``SECURITY_PASSWORD_SALT``, and then with a random salt.

    Default: a list of known schemes not working with double hashing (`django_{digest}`, `plaintext`).

.. py:data:: SECURITY_HASHING_SCHEMES

    List of algorithms used for encrypting/hashing sensitive data within a token
    (Such as is sent with confirmation or reset password).

    Default: ``sha256_crypt``.
.. py:data:: SECURITY_DEPRECATED_HASHING_SCHEMES

    List of deprecated algorithms used for creating and validating tokens.

    Default: ``hex_md5``.

.. py:data:: SECURITY_PASSWORD_HASH_OPTIONS

    Specifies additional options to be passed to the hashing method. This is deprecated as of passlib 1.7.

    .. deprecated:: 3.4.0 see: :py:data:`SECURITY_PASSWORD_HASH_PASSLIB_OPTIONS`

.. py:data:: SECURITY_PASSWORD_HASH_PASSLIB_OPTIONS

    Pass additional options to the various hashing methods. This is a
    dict of the form ``{<scheme>__<option>: <value>, ..}``
    e.g. {"argon2__rounds": 10}.

    .. versionadded:: 3.3.1

.. py:data:: SECURITY_PASSWORD_LENGTH_MIN

    Minimum required length for passwords.

    Default: 8

    .. versionadded:: 3.4.0
.. py:data:: SECURITY_PASSWORD_COMPLEXITY_CHECKER

    Set to complexity checker to use (Only ``zxcvbn`` supported).

    Default: ``None``

    .. versionadded:: 3.4.0
.. py:data:: SECURITY_PASSWORD_CHECK_BREACHED

    If not ``None`` new/changed passwords will be checked against the
    database of breached passwords at https://api.pwnedpasswords.com.
    If set to ``strict`` then if the site can't be reached, validation will fail.
    If set to ``best-effort`` failure to reach the site will continue
    with the rest of password validation.

    Default: ``None``

    .. versionadded:: 3.4.0
.. py:data:: SECURITY_PASSWORD_BREACHED_COUNT

    Passwords with counts greater than or equal to this value are considered breached.

    Default: 1  - which might be to burdensome for some applications.

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_PASSWORD_NORMALIZE_FORM

    Passwords are normalized prior to changing or comparing. This satisfies
    the NIST requirement: `5.1.1.2 Memorized Secret Verifiers`_.
    Normalization is performed using the Python unicodedata.normalize() method.

    Default: "NFKD"

    .. versionadded:: 4.0.0

.. _5.1.1.2 Memorized Secret Verifiers: https://pages.nist.gov/800-63-3/sp800-63b.html#sec5

.. py:data:: SECURITY_TOKEN_AUTHENTICATION_KEY

    Specifies the query string parameter to read when using token authentication.

    Default: ``auth_token``.

.. py:data:: SECURITY_TOKEN_AUTHENTICATION_HEADER

    Specifies the HTTP header to read when using token authentication.

    Default: ``Authentication-Token``.

.. py:data:: SECURITY_TOKEN_MAX_AGE

    Specifies the number of seconds before an authentication token expires.

    Default: ``None``, meaning the token never expires.

.. py:data:: SECURITY_EMAIL_VALIDATOR_ARGS

    Email address are validated using the `email_validator`_ package. Its methods
    have some configurable options - these can be set here and will be passed in.
    For example setting this to: ``{"check_deliverability": False}`` is useful
    when unit testing if the emails are fake.


    Default: ``None``, meaning use the defaults from email_validator package.

    .. versionadded:: 4.0.0

.. _email_validator: https://pypi.org/project/email-validator/

.. py:data:: SECURITY_DEFAULT_HTTP_AUTH_REALM

    Specifies the default authentication realm when using basic HTTP auth.

    Default: ``Login Required``

.. py:data:: SECURITY_REDIRECT_BEHAVIOR

    Passwordless login, confirmation, and reset password have GET endpoints that validate
    the passed token and redirect to an action form.
    For Single-Page-Applications style UIs which need to control their own internal URL routing these redirects
    need to not contain forms, but contain relevant information as query parameters.
    Setting this to ``spa`` will enable that behavior.

    Default: ``None`` which is existing html-style form redirects.

    .. versionadded:: 3.3.0

.. py:data:: SECURITY_REDIRECT_HOST

    Mostly for development purposes, the UI is often developed
    separately and is running on a different port than the
    Flask application. In order to test redirects, the `netloc`
    of the redirect URL needs to be rewritten. Setting this to e.g. `localhost:8080` does that.

    Default: ``None``.

    .. versionadded:: 3.3.0

.. py:data:: SECURITY_REDIRECT_ALLOW_SUBDOMAINS

    If ``True`` then subdomains (and the root domain) of the top-level host set
    by Flask's ``SERVER_NAME`` configuration will be allowed as post-login redirect targets.
    This is beneficial if you wish to place your authentiation on one subdomain and
    authenticated content on another, for example ``auth.domain.tld`` and ``app.domain.tld``.

    Default: ``False``.

    .. versionadded:: 4.0.0

.. py:data:: SECURITY_CSRF_PROTECT_MECHANISMS

    Authentication mechanisms that require CSRF protection.
    These are the same mechanisms as are permitted in the ``@auth_required`` decorator.

    Default: ``("basic", "session", "token")``.

.. py:data:: SECURITY_CSRF_IGNORE_UNAUTH_ENDPOINTS

    If ``True`` then CSRF will not be required for endpoints
    that don't require authentication (e.g. login, logout, register, forgot_password).

    Default: ``False``.

.. py:data:: SECURITY_CSRF_COOKIE

    A dict that defines the parameters required to
    set a CSRF cookie. At a minimum it requires a 'key'.
    The complete set of parameters is described in Flask's `set_cookie`_ documentation.

    Default: ``{"key": None}`` which means no cookie will sent.

.. py:data:: SECURITY_CSRF_HEADER

    The HTTP Header name that will contain the CSRF token. ``X-XSRF-Token``
    is used by packages such as `axios`_.

    Default: ``X-XSRF-Token``.

.. py:data:: SECURITY_CSRF_COOKIE_REFRESH_EACH_REQUEST

    By default, csrf_tokens have an expiration (controlled
    by the configuration variable ``WTF_CSRF_TIME_LIMIT``.
    This can cause CSRF failures if say an application is left
    idle for a long time. You can set that time limit to ``None``
    or have the CSRF cookie sent on every request (which will give
    it a new expiration time).

    Default: ``False``.

.. py:data:: SECURITY_EMAIL_SENDER

    Specifies the email address to send emails as.

    Default: value set to ``MAIL_DEFAULT_SENDER`` if Flask-Mail is used otherwise ``no-reply@localhost``.

.. py:data:: SECURITY_USER_IDENTITY_ATTRIBUTES

    Specifies which attributes of the user object can be used for credential validation.

    Defines the order and matching that will be applied when validating login
    credentials (either via standard login form or the unified sign in form).
    The identity field in the form will be matched in order using this configuration
    - the FIRST match will then be used to look up the user in the DB.

    Mapping functions take a single argument - ``identity`` from the form
    and should return ``None`` if the ``identity`` argument isn't in a format
    suitable for the attribute. If the ``identity`` argument format matches, it
    should be returned, optionally having had some canonicalization performed.
    The returned result will be used to look up the identity in the UserDataStore
    using the column name specified in the key.

    The provided :meth:`flask_security.uia_phone_mapper` for example performs
    phone number normalization using the ``phonenumbers`` package.

    .. tip::
        If your mapper performs any sort of canonicalization/normalization,
        make sure you apply the exact same transformation in your form validator
        when setting the field.

    .. danger::
        Make sure that any attributes listed here are marked Unique in your UserDataStore
        model.

    .. danger::
        Make sure your mapper methods guard against malicious user input. For example,
        if you allow ``username`` as an identity method you could use `bleach`_::

            def uia_username_mapper(identity):
                # we allow pretty much anything - but we bleach it.
                return bleach.clean(identity, strip=True)

    Default::

        [
            {"email": {"mapper": uia_email_mapper, "case_insensitive": True}},
        ]

    If you enable :py:data:`SECURITY_UNIFIED_SIGNIN` and set ``sms`` as a :py:data:`SECURITY_US_ENABLED_METHODS`
    the following would be necessary::

        [
            {"email": {"mapper": uia_email_mapper, "case_insensitive": True}},
            {"us_phone_number": {"mapper": uia_phone_number}},
        ]


    .. versionchanged:: 4.0.0
        Changed from list to list of dict.

.. _bleach: https://pypi.org/project/bleach/

.. py:data:: SECURITY_USER_IDENTITY_MAPPINGS

    .. versionadded:: 3.4.0
    .. deprecated:: 4.0.0
        Superseded by :py:data:`SECURITY_USER_IDENTITY_ATTRIBUTES`

.. py:data:: SECURITY_API_ENABLED_METHODS

    Various endpoints of Flask-Security require the caller to be authenticated.
    This variable controls which of the methods - ``token``, ``session``, ``basic``
    will be allowed. The default does NOT include ``basic`` since if ``basic``
    is in the list, and if the user is NOT authenticated, then the standard/required
    response of 401 with the ``WWW-Authenticate`` header is returned. This is
    rarely what the client wants.

    Default: ``["session", "token"]``.

    .. versionadded:: 4.0.0

.. py:data:: SECURITY_DEFAULT_REMEMBER_ME

    Specifies the default "remember me" value used when logging in a user.

    Default: ``False``.

.. py:data:: SECURITY_BACKWARDS_COMPAT_UNAUTHN

    If set to ``True`` then the default behavior for authentication
    failures from one of Flask-Security's decorators will be restored to
    be compatible with releases prior to 3.3.0 (return 401 and some static html).

    Default: ``False``.

.. py:data:: SECURITY_BACKWARDS_COMPAT_AUTH_TOKEN

    If set to ``True`` then an Authentication-Token will be returned
    on every successful call to login, reset-password, change-password
    as part of the JSON response. This was the default prior to release 3.3.0
    - however sending Authentication-Tokens (which by default don't expire)
    to session based UIs is a bad security practice.

    Default: ``False``.

Core - Multi-factor
-------------------
These are used by the Two-Factor and Unified Signin features.

.. py:data:: SECURITY_TOTP_SECRETS

    Secret used to encrypt the totp_password both into DB and into the session cookie.
    Best practice is to set this to:

    .. code-block:: python

        from passlib import totp
        "{1: <result of totp.generate_secret()>}"

    See: `Totp`_ for details.

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_TOTP_ISSUER

    Specifies the name of the service or application that the user is authenticating to.
    This will be the name displayed by most authenticator apps.

    Default: ``None``.

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_SMS_SERVICE

    Specifies the name of the sms service provider. Out of the box
    "Twilio" is supported. For other sms service providers you will need
    to subclass :class:`.SmsSenderBaseClass` and register it:

    .. code-block:: python

        SmsSenderFactory.senders[<service-name>] = <service-class>

    Default: ``Dummy`` which does nothing.

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_SMS_SERVICE_CONFIG

    Specifies a dictionary of basic configurations needed for use of a sms service.
    For "Twilio" the following keys are required (fill in from your Twilio dashboard):

    Default: ``{'ACCOUNT_SID': NONE, 'AUTH_TOKEN': NONE, 'PHONE_NUMBER': NONE}``

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_PHONE_REGION_DEFAULT

    Assigns a default 'region' for phone numbers used for two-factor or
    unified sign in. All other phone numbers will require a region prefix to
    be accepted.

    Default: ``US``

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_FRESHNESS

    A timedelta used to protect endpoints that alter sensitive information.
    This is used to protect the endpoint: :py:data:`SECURITY_US_SETUP_URL`, and
    :py:data:`SECURITY_TWO_FACTOR_SETUP_URL`.
    Setting this to a negative number will disable any freshness checking and
    the endpoints :py:data:`SECURITY_VERIFY_URL`, :py:data:`SECURITY_US_VERIFY_URL`
    and :py:data:`SECURITY_US_VERIFY_SEND_CODE_URL` won't be registered.
    Setting this to 0 results in undefined behavior.
    Please see :meth:`flask_security.check_and_update_authn_fresh` for details.

    Default: timedelta(hours=24)

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_FRESHNESS_GRACE_PERIOD

    A timedelta that provides a grace period when altering sensitive
    information.
    This is used to protect the endpoint: :py:data:`SECURITY_US_SETUP_URL`, and
    :py:data:`SECURITY_TWO_FACTOR_SETUP_URL`.
    N.B. To avoid strange behavior, be sure to set the grace period less than
    the freshness period.
    Please see :meth:`flask_security.check_and_update_authn_fresh` for details.

    Default: timedelta(hours=1)

    .. versionadded:: 3.4.0


Core - rarely need changing
----------------------------

.. py:data:: SECURITY_DATETIME_FACTORY

    Specifies the default datetime factory.

    Default:``datetime.datetime.utcnow``.

.. py:data:: SECURITY_CONFIRM_SALT

    Specifies the salt value when generating confirmation links/tokens.

    Default: ``"confirm-salt"``.

.. py:data:: SECURITY_RESET_SALT

    Specifies the salt value when generating password reset links/tokens.

    Default: ``"reset-salt"``.

.. py:data:: SECURITY_LOGIN_SALT

    Specifies the salt value when generating login links/tokens.

    Default: ``"login-salt"``.

.. py:data:: SECURITY_REMEMBER_SALT

    Specifies the salt value when generating remember tokens.
    Remember tokens are used instead of user ID's as it is more secure.

    Default: ``"remember-salt"``.
.. py:data:: SECURITY_TWO_FACTOR_VALIDITY_SALT

    Specifies the salt value when generating two factor validity tokens.

    Default: ``"tf-validity-salt"``.
.. py:data:: SECURITY_US_SETUP_SALT

    Default: ``"us-setup-salt"``

.. py:data:: SECURITY_EMAIL_PLAINTEXT

    Sends email as plaintext using ``*.txt`` template.

    Default: ``True``.

.. py:data:: SECURITY_EMAIL_HTML

    Sends email as HTML using ``*.html`` template.

    Default: ``True``.

.. py:data:: SECURITY_CLI_USERS_NAME

    Specifies the name for the command managing users. Disable by setting ``False``.

    Default: ``users``.

.. py:data:: SECURITY_CLI_ROLES_NAME

    Specifies the name for the command managing roles. Disable by setting ``False``.

    Default: ``roles``.

.. py:data:: SECURITY_JOIN_USER_ROLES

    Specifies whether to set the ``UserModel.roles`` loading relationship to ``joined`` when a ``roles`` attribute
    is present for a SQLAlchemy Datastore. Setting this to ``False`` restores pre 3.3.0 behavior and is required if the ``roles`` attribute
    is not a joinable attribute on the ``UserModel``. The default setting improves performance by only requiring a single
    DB call.

    Default: ``True``.

    .. versionadded:: 3.4.0

.. _Totp: https://passlib.readthedocs.io/en/stable/narr/totp-tutorial.html#totp-encryption-setup
.. _set_cookie: https://flask.palletsprojects.com/en/1.1.x/api/?highlight=set_cookie#flask.Response.set_cookie
.. _axios: https://github.com/axios/axios
.. _bcrypt: https://pypi.org/project/bcrypt/
.. _argon2: https://pypi.org/project/argon2-cffi/

Login/Logout
------------
.. py:data:: SECURITY_LOGIN_URL

    Specifies the login URL.

    Default: ``"/login"``.

.. py:data:: SECURITY_LOGOUT_URL

    Specifies the logout URL.

    Default:``"/logout"``.


.. py:data:: SECURITY_LOGOUT_METHODS

    Specifies the HTTP request methods that the logout URL accepts. Specify ``None`` to disable the logout URL (and implement your own).
    Configuring with just ``["POST"]`` is slightly more secure. The default includes ``"GET"`` for backwards compatibility.

    Default: ``["GET", "POST"]``.


.. py:data:: SECURITY_POST_LOGIN_VIEW

    Specifies the default view to redirect to after a user logs in. This value can be set to a URL
    or an endpoint name. Defaults to the Flask config ``APPLICATION_ROOT`` value which itself defaults to ``"/"``.

    Default: ``APPLICATION_ROOT``.

.. py:data:: SECURITY_POST_LOGOUT_VIEW

    Specifies the default view to redirect to after a user logs out. This value can be set to a URL
    or an endpoint name. Defaults to the Flask config ``APPLICATION_ROOT`` value which itself defaults to ``"/"``.

    Default: ``APPLICATION_ROOT``.


.. py:data:: SECURITY_UNAUTHORIZED_VIEW

    Specifies the view to redirect to if a user attempts to access a URL/endpoint that they do
    not have permission to access. If this value is ``None``, the user is presented with a default
    HTTP 403 response.

    Default: ``None``.

.. py:data:: SECURITY_LOGIN_USER_TEMPLATE

    Specifies the path to the template for the user login page.

    Default: ``"security/login_user.html"``.

.. py:data:: SECURITY_VERIFY_URL

    Specifies the re-authenticate URL. If :py:data:`SECURITY_FRESHNESS` evaluates to < 0; this
    endpoint won't be registered.

    Default: ``"/verify"``


.. py:data:: SECURITY_VERIFY_TEMPLATE

    Specifies the path to the template for the verify password page.

    Default: ``"security/verify.html"``.

.. py:data:: SECURITY_POST_VERIFY_URL

    Specifies the default view to redirect to after a user successfully re-authenticates either via
    the :py:data:`SECURITY_VERIFY_URL` or the :py:data:`SECURITY_US_VERIFY_URL`.
    Normally this won't need to be set and after the verification/re-authentication, the referring
    view (held in the ``next`` parameter) will be redirected to.

    Default: ``None``.

Registerable
------------
.. py:data:: SECURITY_REGISTERABLE

    Specifies if Flask-Security should create a user registration endpoint.

    Default: ``False``

.. py:data:: SECURITY_SEND_REGISTER_EMAIL

    Specifies whether registration email is sent.

    Default: ``True``.
.. py:data:: SECURITY_EMAIL_SUBJECT_REGISTER

    Sets the subject for the confirmation email.

    Default: ``Welcome``.
.. py:data:: SECURITY_REGISTER_USER_TEMPLATE

    Specifies the path to the template for the user registration page.

    Default: ``security/register_user.html``.
.. py:data:: SECURITY_POST_REGISTER_VIEW

    Specifies the view to redirect to after a user successfully registers.
    This value can be set to a URL or an endpoint name. If this value is
    ``None``, the user is redirected to the value of ``SECURITY_POST_LOGIN_VIEW``.

    Default: ``None``.
.. py:data:: SECURITY_REGISTER_URL

    Specifies the register URL.

    Default: ``"/register"``.

Confirmable
-----------

.. py:data:: SECURITY_CONFIRMABLE

    Specifies if users are required to confirm their email address when
    registering a new account. If this value is `True`, Flask-Security creates an endpoint to handle
    confirmations and requests to resend confirmation instructions.

    Default: ``False``.
.. py:data:: SECURITY_CONFIRM_EMAIL_WITHIN

    Specifies the amount of time a user has before their confirmation
    link expires. Always pluralize the time unit for this value.

    Default: ``5 days``.
.. py:data:: SECURITY_CONFIRM_URL

    Specifies the email confirmation URL.

    Default: ``"/confirm"``.
.. py:data:: SECURITY_SEND_CONFIRMATION_TEMPLATE

    Specifies the path to the template for the resend confirmation instructions page.

    Default: ``security/send_confirmation.html``.
.. py:data:: SECURITY_EMAIL_SUBJECT_CONFIRM

    Sets the subject for the email confirmation message.

    Default: ``Please confirm your email``.
.. py:data:: SECURITY_CONFIRM_ERROR_VIEW

    Specifies the view to redirect to if a confirmation error occurs.
    This value can be set to a URL or an endpoint name.
    If this value is ``None``, the user is presented the default view
    to resend a confirmation link. In the case of ``SECURITY_REDIRECT_BEHAVIOR`` == ``spa``
    query params in the redirect will contain the error.

    Default: ``None``.
.. py:data:: SECURITY_POST_CONFIRM_VIEW

    Specifies the view to redirect to after a user successfully confirms their email.
    This value can be set to a URL or an endpoint name. If this value is ``None``, the user is redirected to the
    value of ``SECURITY_POST_LOGIN_VIEW``.

    Default: ``None``.
.. py:data:: SECURITY_AUTO_LOGIN_AFTER_CONFIRM

    If ``False`` then on confirmation  the user will be required to login again.
    Note that the confirmation token is not valid after being used once.
    If ``True``, then the user corresponding to the
    confirmation token will be automatically logged in.

    Default: ``True``.
.. py:data:: SECURITY_LOGIN_WITHOUT_CONFIRMATION

    Specifies if a user may login before confirming their email when
    the value of ``SECURITY_CONFIRMABLE`` is set to ``True``.

    Default: ``False``.
.. py:data:: SECURITY_REQUIRES_CONFIRMATION_ERROR_VIEW

    Specifies a redirect page if the users tries to login, reset password or us-signin with an unconfirmed account.
    If an URL endpoint is specified, flashes an error messages and passes user email as an argument.
    For us-signin, no argument is specified: it simply flashes the error message and redirects.
    Default behavior is to reload the form with an error message without redirecting to an other page.

    Default: ``None``.

Changeable
----------
Configuration variables for the ``SECURITY_CHANGEABLE`` feature:

.. py:data:: SECURITY_CHANGEABLE

    Specifies if Flask-Security should enable the change password endpoint.

    Default: ``False``.
.. py:data:: SECURITY_CHANGE_URL

    Specifies the password change URL.

    Default: ``"/change"``.
.. py:data:: SECURITY_POST_CHANGE_VIEW

    Specifies the view to redirect to after a user successfully changes their password.
    This value can be set to a URL or an endpoint name.
    If this value is ``None``, the user is redirected  to the
    value of ``SECURITY_POST_LOGIN_VIEW``.

    Default: ``None``.
.. py:data:: SECURITY_CHANGE_PASSWORD_TEMPLATE

    Specifies the path to the template for the change password page.

    Default: ``security/change_password.html``.

.. py:data:: SECURITY_SEND_PASSWORD_CHANGE_EMAIL

    Specifies whether password change email is sent.

    Default: ``True``.

.. py:data:: SECURITY_EMAIL_SUBJECT_PASSWORD_CHANGE_NOTICE

    Sets the subject for the password change notice.

    Default: ``Your password has been changed``.

Recoverable
-----------

.. py:data:: SECURITY_RECOVERABLE

    Specifies if Flask-Security should create a password reset/recover endpoint.

    Default: ``False``.

.. py:data:: SECURITY_RESET_URL

    Specifies the password reset URL.

    Default: ``"/reset"``.

.. py:data:: SECURITY_RESET_PASSWORD_TEMPLATE

    Specifies the path to the template for the reset password page.

    Default: ``security/reset_password.html``.

.. py:data:: SECURITY_FORGOT_PASSWORD_TEMPLATE

    Specifies the path to the template for the forgot password page.

    Default: ``security/forgot_password.html``.

.. py:data:: SECURITY_POST_RESET_VIEW

    Specifies the view to redirect to after a user successfully resets their password.
    This value can be set to a URL or an endpoint name. If this
    value is ``None``, the user is redirected  to the value of ``SECURITY_POST_LOGIN_VIEW``.

    Default: ``None``.

.. py:data:: SECURITY_RESET_VIEW

    Specifies the view/URL to redirect to after a GET reset-password link.
    This is only valid if ``SECURITY_REDIRECT_BEHAVIOR`` == ``spa``.
    Query params in the redirect will contain the ``token`` and ``email``.

    Default: ``None``.

.. py:data:: SECURITY_RESET_ERROR_VIEW

    Specifies the view/URL to redirect to after a GET reset-password link when there is an error.
    This is only valid if ``SECURITY_REDIRECT_BEHAVIOR`` == ``spa``.
    Query params in the redirect will contain the error.

    Default: ``None``.

.. py:data:: SECURITY_RESET_PASSWORD_WITHIN

    Specifies the amount of time a user has before their password reset link expires.
    Always pluralize the time unit for this value.

    Default: ``5 days``.

.. py:data:: SECURITY_SEND_PASSWORD_RESET_EMAIL

    Specifies whether password reset email is sent. These are instructions
    including a link that can be clicked on.

    Default: ``True``.

.. py:data:: SECURITY_SEND_PASSWORD_RESET_NOTICE_EMAIL

    Specifies whether password reset notice email is sent. This is sent once
    a user's password was successfully reset.

    Default: ``True``.

.. py:data:: SECURITY_EMAIL_SUBJECT_PASSWORD_RESET

    Sets the subject for the password reset email.

    Default: ``Password reset instructions``.

.. py:data:: SECURITY_EMAIL_SUBJECT_PASSWORD_NOTICE

    Sets subject for the password notice.

    Default: ``Your password has been reset``.

Two-Factor
-----------
Configuration related to the two-factor authentication feature.

.. versionadded:: 3.2.0

.. py:data:: SECURITY_TWO_FACTOR

    Specifies if Flask-Security should enable the two-factor login feature.
    If set to ``True``, in addition to their passwords, users will be required to
    enter a code that is sent to them. Note that unless
    ``SECURITY_TWO_FACTOR_REQUIRED`` is set - this is opt-in.

    Default: ``False``.
.. py:data:: SECURITY_TWO_FACTOR_REQUIRED

    If set to ``True`` then all users will be required to setup and use two factor authorization.

    Default: ``False``.
.. py:data:: SECURITY_TWO_FACTOR_ENABLED_METHODS

    Specifies the default enabled methods for two-factor authentication.

    Default: ``['email', 'authenticator', 'sms']`` which are the only currently supported methods.

.. py:data:: SECURITY_TWO_FACTOR_SECRET

    .. deprecated:: 3.4.0 see: :py:data:`SECURITY_TOTP_SECRETS`

.. py:data:: SECURITY_TWO_FACTOR_URI_SERVICE_NAME

    .. deprecated:: 3.4.0 see: :py:data:`SECURITY_TOTP_ISSUER`

.. py:data:: SECURITY_TWO_FACTOR_SMS_SERVICE

    .. deprecated:: 3.4.0 see: :py:data:`SECURITY_SMS_SERVICE`

.. py:data:: SECURITY_TWO_FACTOR_SMS_SERVICE_CONFIG

    .. deprecated:: 3.4.0 see: :py:data:`SECURITY_SMS_SERVICE_CONFIG`

.. py:data:: SECURITY_TWO_FACTOR_AUTHENTICATOR_VALIDITY

    Specifies the number of seconds access token is valid.

    Default: ``120``.
.. py:data:: SECURITY_TWO_FACTOR_MAIL_VALIDITY

    Specifies the number of seconds access token is valid.

    Default: ``300``.
.. py:data:: SECURITY_TWO_FACTOR_SMS_VALIDITY

    Specifies the number of seconds access token is valid.

    Default: ``120``.
.. py:data:: SECURITY_TWO_FACTOR_RESCUE_MAIL

    Specifies the email address users send mail to when they can't complete the
    two-factor authentication login.

    Default: ``no-reply@localhost``.

.. py:data:: SECURITY_EMAIL_SUBJECT_TWO_FACTOR

    Sets the subject for the two factor feature.

    Default: ``Two-factor Login``
.. py:data:: SECURITY_EMAIL_SUBJECT_TWO_FACTOR_RESCUE

    Sets the subject for the two factor help function.

    Default: ``Two-factor Rescue``
.. py:data:: SECURITY_TWO_FACTOR_VERIFY_CODE_TEMPLATE

    Specifies the path to the template for the verify code page for the two-factor authentication process.

    Default: ``security/two_factor_verify_code.html``.
.. py:data:: SECURITY_TWO_FACTOR_SETUP_TEMPLATE

    Specifies the path to the template for the setup page for the two factor authentication process.

    Default: ``security/two_factor_setup.html``.

.. py:data:: SECURITY_TWO_FACTOR_SETUP_URL

    Specifies the two factor setup URL.

    Default: ``"/tf-setup"``.
.. py:data:: SECURITY_TWO_FACTOR_TOKEN_VALIDATION_URL

    Specifies the two factor token validation URL.

    Default: ``"/tf-validate"``.

.. py:data:: SECURITY_TWO_FACTOR_RESCUE_URL

    Specifies the two factor rescue URL.

    Default: ``"/tf-rescue"``.

.. py:data:: SECURITY_TWO_FACTOR_ALWAYS_VALIDATE

    Specifies whether the application should require a two factor code upon every login.
    If set to ``False`` then the 2 values below are used to determine when
    a code is required. Note that this is cookie based - so a new browser
    session will always require a fresh two-factor code.

    Default: ``True``.
.. py:data:: SECURITY_TWO_FACTOR_LOGIN_VALIDITY

    Specifies the expiration of the two factor validity cookie and verification of the token.

    Default: ``30 Days``.


.. py:data:: SECURITY_TWO_FACTOR_VALIDITY_COOKIE

    A dictionary containing the parameters of the two factor validity cookie.
    The complete set of parameters is described in Flask's `set_cookie`_ documentation.

    Default: ``{'httponly': True, 'secure': False, 'samesite': None}``.


Unified Signin
--------------

    .. versionadded:: 3.4.0

.. py:data:: SECURITY_UNIFIED_SIGNIN

    To enable this feature - set this to ``True``.

    Default: ``False``

.. py:data:: SECURITY_US_SIGNIN_URL

    Sign in a user with an identity and a passcode.

    Default: ``"/us-signin"``

.. py:data:: SECURITY_US_SIGNIN_SEND_CODE_URL

    Endpoint that given an identity, and a previously setup authentication method, will
    generate and return a one time code. This isn't necessary when using an authenticator app.

    Default: ``"/us-signin/send-code"``

.. py:data:: SECURITY_US_SETUP_URL

    Endpoint for setting up and validating SMS or an authenticator app for use in
    receiving one-time codes.

    Default: ``"/us-setup"``

.. py:data:: SECURITY_US_VERIFY_LINK_URL

    This endpoint handles the 'magic link' that is sent when the user requests a code
    via email. It is mostly just accessed via a ``GET`` from an email reader.

    Default: ``"/us-verify-link"``

.. py:data:: SECURITY_US_VERIFY_URL

    This endpoint handles re-authentication, the caller must be already authenticated
    and then enter in their primary credentials (password/passcode) again. This is
    used when an endpoint (such as ``/us-setup``) fails freshness checks.
    This endpoint won't be registered if :py:data:`SECURITY_FRESHNESS` evaluates to < 0.

    Default: ``"/us-verify"``

.. py:data:: SECURITY_US_VERIFY_SEND_CODE_URL

    As part of ``/us-verify``, this endpoint will send the appropriate code.
    This endpoint won't be registered if :py:data:`SECURITY_FRESHNESS` evaluates to < 0.

    Default: ``"/us-verify/send-code"``

.. py:data:: SECURITY_US_POST_SETUP_VIEW

    Specifies the view to redirect to after a user successfully setups an authentication method (non-json).
    This value can be set to a URL or an endpoint name. If this value is ``None``, the user is redirected to the
    value of :py:data:`SECURITY_POST_LOGIN_VIEW`.

    Default: ``None``

.. py:data:: SECURITY_US_SIGNIN_TEMPLATE

    Default: ``"security/us_signin.html"``

.. py:data:: SECURITY_US_SETUP_TEMPLATE

    Default: ``"security/us_setup.html"``

.. py:data:: SECURITY_US_VERIFY_TEMPLATE

    Default: ``"security/us_verify.html"``

.. py:data:: SECURITY_US_ENABLED_METHODS

    Specifies the default enabled methods for unified sign in authentication.
    Be aware that ``password`` only affects this ``SECURITY_US_SIGNIN_URL`` endpoint.
    Removing it from here won't stop users from using the ``SECURITY_LOGIN_URL`` endpoint.

    If you select ``sms`` then make sure you add this to :py:data:`SECURITY_USER_IDENTITY_ATTRIBUTES`::

        {"us_phone_number": {"mapper": uia_phone_number}},


    Default: ``["password", "email", "authenticator", "sms"]`` - which are the only supported options.

.. py:data:: SECURITY_US_MFA_REQUIRED

    A list of ``US_ENABLED_METHODS`` that will require two-factor
    authentication. This is of course dependent on the settings of :py:data:`SECURITY_TWO_FACTOR`
    and :py:data:`SECURITY_TWO_FACTOR_REQUIRED`. Note that even with REQUIRED, only
    methods listed here will trigger a two-factor cycle.

    Default: ``["password", "email"]``.

.. py:data:: SECURITY_US_TOKEN_VALIDITY

    Specifies the number of seconds access token/code is valid.

    Default: ``120``

.. py:data:: SECURITY_US_EMAIL_SUBJECT

    Sets the email subject when sending the verification code via email.

    Default: ``_("Verification Code")``

.. py:data:: SECURITY_US_SETUP_WITHIN

    Specifies the amount of time a user has before their setup
    token expires. Always pluralize the time unit for this value.

    Default: "30 minutes"

.. py:data:: SECURITY_US_SIGNIN_REPLACES_LOGIN

    If set, then the :py:data:`SECURITY_LOGIN_URL` will be registered to the ``us-signin`` endpoint.
    Doing this will mean that logout will properly redirect to the us-signin endpoint.

    Default: ``False``


Additional relevant configuration variables:

    * :py:data:`SECURITY_USER_IDENTITY_ATTRIBUTES` - Defines the order and methods for parsing and validating identity.
    * :py:data:`SECURITY_DEFAULT_REMEMBER_ME`
    * :py:data:`SECURITY_SMS_SERVICE` - When SMS is enabled in :py:data:`SECURITY_US_ENABLED_METHODS`.
    * :py:data:`SECURITY_SMS_SERVICE_CONFIG`
    * :py:data:`SECURITY_TOTP_SECRETS`
    * :py:data:`SECURITY_TOTP_ISSUER`
    * :py:data:`SECURITY_PHONE_REGION_DEFAULT`
    * :py:data:`SECURITY_LOGIN_ERROR_VIEW` - The user is redirected here if
      :py:data:`SECURITY_US_VERIFY_LINK_URL` has an error and the request is json and
      :py:data:`SECURITY_REDIRECT_BEHAVIOR` equals ``"spa"``.
    * :py:data:`SECURITY_FRESHNESS` - Used to protect /us-setup.
    * :py:data:`SECURITY_FRESHNESS_GRACE_PERIOD` - Used to protect /us-setup.

Passwordless
-------------

.. py:data:: SECURITY_PASSWORDLESS

    Specifies if Flask-Security should enable the passwordless login feature.
    If set to ``True``, users are not required to enter a password to login but are
    sent an email with a login link.
    **This feature is being replaced with a more generalized passwordless feature
    that includes using SMS or authenticator applications for generating codes.**

    Default: ``False``.

.. py:data:: SECURITY_SEND_LOGIN_TEMPLATE

    Specifies the path to the template for the send login instructions page for
    passwordless logins.

    Default:``security/send_login.html``.

.. py:data:: SECURITY_EMAIL_SUBJECT_PASSWORDLESS

    Sets the subject for the passwordless feature.

    Default: ``Login instructions``.

.. py:data:: SECURITY_LOGIN_WITHIN

    Specifies the amount of time a user has before a login link expires.
    Always pluralize the time unit for this value.

    Default: ``1 days``.

.. py:data:: SECURITY_LOGIN_ERROR_VIEW

    Specifies the view/URL to redirect to after a GET passwordless link or GET
    unified sign in magic link when there is an error.
    This is only valid if ``SECURITY_REDIRECT_BEHAVIOR`` == ``spa``.
    Query params in the redirect will contain the error.

    Default: ``None``.

Trackable
----------
.. py:data:: SECURITY_TRACKABLE

    Specifies if Flask-Security should track basic user login statistics. If set to ``True``, ensure your
    models have the required fields/attributes and make sure to commit changes after calling
    ``login_user``. Be sure to use `ProxyFix <http://flask.pocoo.org/docs/0.10/deploying/wsgi-standalone/#proxy-setups>`_ if you are using a proxy.

    Default: ``False``

Feature Flags
-------------
All feature flags. By default all are 'False'/not enabled.

* ``SECURITY_CONFIRMABLE``
* ``SECURITY_REGISTERABLE``
* ``SECURITY_RECOVERABLE``
* ``SECURITY_TRACKABLE``
* ``SECURITY_PASSWORDLESS``
* ``SECURITY_CHANGEABLE``
* ``SECURITY_TWO_FACTOR``
* :py:data:`SECURITY_UNIFIED_SIGNIN`

URLs and Views
--------------
A list of all URLs and Views:

* ``SECURITY_LOGIN_URL``
* ``SECURITY_LOGOUT_URL``
* :py:data:`SECURITY_VERIFY_URL`
* ``SECURITY_REGISTER_URL``
* ``SECURITY_RESET_URL``
* ``SECURITY_CHANGE_URL``
* ``SECURITY_CONFIRM_URL``
* ``SECURITY_TWO_FACTOR_SETUP_URL``
* ``SECURITY_TWO_FACTOR_TOKEN_VALIDATION_URL``
* ``SECURITY_TWO_FACTOR_RESCUE_URL``
* ``SECURITY_POST_LOGIN_VIEW``
* ``SECURITY_POST_LOGOUT_VIEW``
* ``SECURITY_CONFIRM_ERROR_VIEW``
* ``SECURITY_POST_REGISTER_VIEW``
* ``SECURITY_POST_CONFIRM_VIEW``
* ``SECURITY_POST_RESET_VIEW``
* ``SECURITY_POST_CHANGE_VIEW``
* ``SECURITY_UNAUTHORIZED_VIEW``
* ``SECURITY_RESET_VIEW``
* ``SECURITY_RESET_ERROR_VIEW``
* ``SECURITY_LOGIN_ERROR_VIEW``
* :py:data:`SECURITY_US_SIGNIN_URL`
* :py:data:`SECURITY_US_SETUP_URL`
* :py:data:`SECURITY_US_SIGNIN_SEND_CODE_URL`
* :py:data:`SECURITY_US_VERIFY_LINK_URL`
* :py:data:`SECURITY_US_VERIFY_URL`
* :py:data:`SECURITY_US_VERIFY_SEND_CODE_URL`
* :py:data:`SECURITY_US_POST_SETUP_VIEW`

Template Paths
--------------
A list of all templates:

* ``SECURITY_FORGOT_PASSWORD_TEMPLATE``
* ``SECURITY_LOGIN_USER_TEMPLATE``
* :py:data:`SECURITY_VERIFY_TEMPLATE`
* ``SECURITY_REGISTER_USER_TEMPLATE``
* ``SECURITY_RESET_PASSWORD_TEMPLATE``
* ``SECURITY_CHANGE_PASSWORD_TEMPLATE``
* ``SECURITY_SEND_CONFIRMATION_TEMPLATE``
* ``SECURITY_SEND_LOGIN_TEMPLATE``
* ``SECURITY_TWO_FACTOR_VERIFY_CODE_TEMPLATE``
* ``SECURITY_TWO_FACTOR_SETUP_TEMPLATE``
* :py:data:`SECURITY_US_SIGNIN_TEMPLATE`
* :py:data:`SECURITY_US_SETUP_TEMPLATE`
* :py:data:`SECURITY_US_VERIFY_TEMPLATE`

Messages
-------------

The following are the messages Flask-Security uses.  They are tuples; the first
element is the message and the second element is the error level.

The default messages and error levels can be found in ``core.py``.

* ``SECURITY_MSG_ALREADY_CONFIRMED``
* ``SECURITY_MSG_API_ERROR``
* ``SECURITY_MSG_ANONYMOUS_USER_REQUIRED``
* ``SECURITY_MSG_CONFIRMATION_EXPIRED``
* ``SECURITY_MSG_CONFIRMATION_REQUEST``
* ``SECURITY_MSG_CONFIRMATION_REQUIRED``
* ``SECURITY_MSG_CONFIRM_REGISTRATION``
* ``SECURITY_MSG_DISABLED_ACCOUNT``
* ``SECURITY_MSG_EMAIL_ALREADY_ASSOCIATED``
* ``SECURITY_MSG_EMAIL_CONFIRMED``
* ``SECURITY_MSG_EMAIL_NOT_PROVIDED``
* ``SECURITY_MSG_FAILED_TO_SEND_CODE``
* ``SECURITY_MSG_FORGOT_PASSWORD``
* ``SECURITY_MSG_IDENTITY_ALREADY_ASSOCIATED``
* ``SECURITY_MSG_INVALID_CODE``
* ``SECURITY_MSG_INVALID_CONFIRMATION_TOKEN``
* ``SECURITY_MSG_INVALID_EMAIL_ADDRESS``
* ``SECURITY_MSG_INVALID_LOGIN_TOKEN``
* ``SECURITY_MSG_INVALID_PASSWORD``
* ``SECURITY_MSG_INVALID_PASSWORD_CODE``
* ``SECURITY_MSG_INVALID_REDIRECT``
* ``SECURITY_MSG_INVALID_RESET_PASSWORD_TOKEN``
* ``SECURITY_MSG_LOGIN``
* ``SECURITY_MSG_LOGIN_EMAIL_SENT``
* ``SECURITY_MSG_LOGIN_EXPIRED``
* ``SECURITY_MSG_PASSWORDLESS_LOGIN_SUCCESSFUL``
* ``SECURITY_MSG_PASSWORD_BREACHED``
* ``SECURITY_MSG_PASSWORD_BREACHED_SITE_ERROR``
* ``SECURITY_MSG_PASSWORD_CHANGE``
* ``SECURITY_MSG_PASSWORD_INVALID_LENGTH``
* ``SECURITY_MSG_PASSWORD_IS_THE_SAME``
* ``SECURITY_MSG_PASSWORD_MISMATCH``
* ``SECURITY_MSG_PASSWORD_NOT_PROVIDED``
* ``SECURITY_MSG_PASSWORD_NOT_SET``
* ``SECURITY_MSG_PASSWORD_RESET``
* ``SECURITY_MSG_PASSWORD_RESET_EXPIRED``
* ``SECURITY_MSG_PASSWORD_RESET_REQUEST``
* ``SECURITY_MSG_PASSWORD_TOO_SIMPLE``
* ``SECURITY_MSG_PHONE_INVALID``
* ``SECURITY_MSG_REAUTHENTICATION_REQUIRED``
* ``SECURITY_MSG_REAUTHENTICATION_SUCCESSFUL``
* ``SECURITY_MSG_REFRESH``
* ``SECURITY_MSG_RETYPE_PASSWORD_MISMATCH``
* ``SECURITY_MSG_TWO_FACTOR_INVALID_TOKEN``
* ``SECURITY_MSG_TWO_FACTOR_LOGIN_SUCCESSFUL``
* ``SECURITY_MSG_TWO_FACTOR_CHANGE_METHOD_SUCCESSFUL``
* ``SECURITY_MSG_TWO_FACTOR_PERMISSION_DENIED``
* ``SECURITY_MSG_TWO_FACTOR_METHOD_NOT_AVAILABLE``
* ``SECURITY_MSG_TWO_FACTOR_DISABLED``
* ``SECURITY_MSG_UNAUTHORIZED``
* ``SECURITY_MSG_UNAUTHENTICATED``
* ``SECURITY_MSG_US_METHOD_NOT_AVAILABLE``
* ``SECURITY_MSG_US_SETUP_EXPIRED``
* ``SECURITY_MSG_US_SETUP_SUCCESSFUL``
* ``SECURITY_MSG_US_SPECIFY_IDENTITY``
* ``SECURITY_MSG_USE_CODE``
* ``SECURITY_MSG_USER_DOES_NOT_EXIST``

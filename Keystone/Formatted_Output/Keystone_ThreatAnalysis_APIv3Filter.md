Keystone Threat Modeling : API_V3 Filters
=========================================
### Table of contents
[System Overview](#system)

[Implementation Overview](#implementation)

[System Assumptions](#assumption)

[Data Flow Diagrams](#dfd)

[Entry Points](#entry)

[Assets](#asset)

[Threats](#threats)

<a name="system"/>
###System Overview
####Application version
Keystone Havana Stable Release.
   
####Application Description
Keystone consist of WSGI application. We look into the filters for the api_v3 pipleline.

####Additional Info


<a name="implementation"/>
###Implementation Overview
####Major Components

The default filters present in the api_v3 pipeline are:

1) sizelimit 

2) url_normalize 

3) build_auth_context 

4) token_auth 

5) admin_token_auth 

6) xml_body_v3 

7) json_body 

8) ec2_extension_v3 

9) s3_extension 

10) simple_cert_extension 

11) service_v3 (not covered in this doc)
    
####Dependent components


####Description

###**sizelimit:** keystone.middleware:RequestBodySizeLimiter.factory

Description: Limits the size of an incoming request

Parameters: request

Returns: application

Method:

1. If request.content_length > CONF.max_request_body_size then it raises an exception 
called exception.RequestTooLarge.

2. If request.content_length is None and req.is_body_readable then it limits the size of an 
incoming request by CONF.max_request_body_size

Default CONF.max_request_body_size=114688

###**url_normalize:** keystone.middleware:NormalizingFilter.factory

Description: Middleware filter to handle URL normalization

Parameters: request

Returns: Makes changes to request.environ['PATH_INFO'] if required.

Method:

1. Removes a trailing slash from the given path, if any.

Code Snippet:

` if (len(request.environ['PATH_INFO']) > 1 and
                request.environ['PATH_INFO'][-1] == '/'):
            request.environ['PATH_INFO'] = request.environ['PATH_INFO'][:-1] `

2. Rewrites path to root if no path is given.

Code Snippet:

 ` elif not request.environ['PATH_INFO']:
            request.environ['PATH_INFO'] = '/' `

###**build_auth_context:** keystone.middleware:AuthContextMiddleware.factory

Description: Build the authentication context from the request auth token.

Parameters: request

Returns: Token Auth Context

Exception: Exception.Unauthorized

Method:

1.  Gets the token_id from token_id = request.headers.get(AUTH_TOKEN_HEADER).

1.1 If the token_id is  admin_token does not process further and returns as admin_token 
is handled by class AdminTokenAuthMiddleware.


2. Else, gets token reference using token api, token_ref = self.token_api.get_token(token_id)
(get_token() returns token without validation, mayb be from cache)

3. If you are using the Revoke extension with backend other than KVS then validate token.
(if revocation extenstion, the extension might not nullify the token when revocked, so perform
extra validation)

Code Snippet:

`if not CONF.token.revoke_by_id:
                self.token_api.token_provider_api.validate_token(
                    context['token_id'])`

4. Validates token_bind;  wsgi.validate_token_bind(context, token_ref)

It also set context:

`context = {'token_id': token_id}
context['environment'] = request.environ`

5. Build Auth Context from token and returns the Auth Context

```
Environment variable used to convey the Keystone auth context.
Auth context is essentially the user credential used for policy enforcement.
It is a dictionary with the following attributes:

* ``user_id``: user ID of the principal
* ``project_id`` (optional): project ID of the scoped project if auth is
                             project-scoped
* ``domain_id`` (optional): domain ID of the scoped domain if auth is
                            domain-scoped
* ``roles`` (optional): list of role names for the given scope
* ``group_ids``: list of group IDs for which the API user has membership

```


###**admin_token_auth:**  keystone.middleware:AdminTokenAuthMiddleware.factory

Description: A trivial filter that checks for a pre-defined admin token. Sets 'is_admin' 
to true in the context, expected to be checked by methods that are admin-only.

Parameters: request

Returns: 

Method:

1. Get the token from the AUTH_TOKEN_HEADER;

`token = request.headers.get(AUTH_TOKEN_HEADER)`

2. Gets the context from the request environment; 

`context = request.environ.get(CONTEXT_ENV, {})`

3. If token is admin_token then set CONTEXT_ENV as context['is_admin']  

Code Snippet

`context['is_admin'] = (token == CONF.admin_token)
  request.environ[CONTEXT_ENV] = context`

###**ec2_extension_v3:** keystone.contrib.ec2:Ec2ExtensionV3.factory

Description: This service provides EC2 token validation and CRUD for services configured
with the EC2_token middleware.

Parameters: mapper 

Returns:

Method:

1. Gets an instance of the ec2 controller: `ec2_controller = controllers.Ec2ControllerV3()`

2. Provides Validation and CRUD operations.

Validation Code Snippet: 
 
 `mapper.connect(
            '/ec2tokens',
            controller=ec2_controller,
            action='authenticate',
            conditions=dict(method=['POST']))`

CRUD Code Snippet:
 
  `mapper.connect(
            '/users/{user_id}/credentials/OS-EC2',
            controller=ec2_controller,
            action='ec2_create_credential',
            conditions=dict(method=['POST']))`


###**s3_extension:** keystone.contrib.s3:S3Extension.factory

Description: This service provides S3 token validation for services configured with the 
s3_token middleware to authorize S3 requests.

Parameters: mapper

Returns:

Method:

1. Gets an instance of the ec2 controller; `ec2_controller = controllers.Ec2ControllerV3()`

2. Provides Validation operations.

 `mapper.connect('/s3tokens',
                       controller=controller,
                       action='authenticate',
                       conditions=dict(method=['POST']))`


###**json_body:** 
``` 
    Middleware to allow method arguments to be passed as serialized JSON.

    Accepting arguments as JSON is useful for accepting data that may be more
    complex than simple primitives.

    In this case we accept it as urlencoded data under the key 'json' as in
    json=<urlencoded_json> but this could be extended to accept raw JSON
    in the POST body.

    Filters out the parameters `self`, `context` and anything beginning with
    an underscore.

```

<a name="assumption"/>

###System Assumptions (External Dependencies)
 - The paste configuration file and keystone.conf are configued with due diligence. 

   
###Security Objective
 -input/out sanitization
 -authenticate auth_context
 
<a name="dfd"/>
###Data Flow Diagrams 
####Request Flow through all the filters in the api_v3 pipeline.
![enter image description here][1]

<a name="entry"/>
###Entry Points:

####WSGI application pipeline

----------
<a name="asset"/>
###Assets

8. Token

26. Configuration parameters:

    -CONF.max_request_body_size,
    -request.environ['PATH_INFO'],
    -CONF.admin_token


Intermediary Asset:

-Auth Context

-context[token]

-context[is_admin]

-AUTH_TOKEN_HEADER


Full assets list is documented in url
[Asset Library][2]

----------
<a name="threats"/>
###Threats

####API_v3-01
Threat: Admin token (is_admin) abuse

Threat Agent:
>

Attack Vectors:
>

Security Weakness:
>

Vulnerable Component:
>

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info:

> Comments:

Note: 

1. Is there any other input/output filtering mechanism missing ?
2. Is there a way to tamper authorization context header
3. context[is_admin] is a powerful context, is there a way to exploit it by authorized user.



  [1]:images/DFD_Apiv3_Filter.png
  [2]:Keystone_asset_library.md
  

###Keystone Asset Library

```html
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="36" valign="top">
                <p>
                    <strong>ID</strong>
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    <strong>Name</strong>
                </p>
            </td>
            <td width="168" colspan="4" valign="top">
                <p>
                    <strong>Details</strong>
                </p>
            </td>
            <td width="116" valign="top">
                <p>
                    <strong>Accessible To</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="426" colspan="7" valign="top">
                <p>
                    USER RELATED
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User_id - Unique within the system
                </p>
                <p>
                    User_name – unique within domain
                </p>
                <p>
                    Domin_id – unique within the system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    1.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User secret
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User password – only hash password stored
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    (3) Owner
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    1.2
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User sensitive data
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    1.3
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User protected data
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User_id - Unique within the system
                </p>
                <p>
                    User_name – unique within domain
                </p>
                <p>
                    Domin_id – unique within the system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    R by:
                </p>
                <p>
                    3. Owner
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
                <p>
                    13. OpenStack service user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    2
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Project
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Project_id – unique within system
                </p>
                <p>
                    Project_name-unique within domain
                </p>
                <p>
                    Domain_id-unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    2.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Project Protected credentials
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Project_id – unique within system
                </p>
                <p>
                    Project_name-unique within domain
                </p>
                <p>
                    Domain_id-unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    Viewable to:
                </p>
                <p>
                    3. Owner
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
                <p>
                    13. OpenStack service user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    3
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Group
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    group_id – unique within system
                </p>
                <p>
                    group_name-unique within domain
                </p>
                <p>
                    Domain_id-unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    3.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Group protected credentials
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    group_id – unique within system
                </p>
                <p>
                    group_name-unique within domain
                </p>
                <p>
                    Domain_id-unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    Viewable to:
                </p>
                <p>
                    3. Owner
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
                <p>
                    13. OpenStack service user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    3.2
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    CRUD user in Group
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User_id
                </p>
                <p>
                    Group_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    4
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Role
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Role_id – unique within system
                </p>
                <p>
                    Role_name – unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CUD by
                </p>
                <p>
                    6. Cloud Admin
                </p>
                <p>
                    R by
                </p>
                <p>
                    7. Admin role also
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    5.
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    domain
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    doman_id – unique within system
                </p>
                <p>
                    domain_name-unique within system
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    6.
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Grant
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    6.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User-project
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User_id
                </p>
                <p>
                    Project_id
                </p>
                <p>
                    Role_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    4. Project Admin
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    6.2
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    User-domain
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User_id
                </p>
                <p>
                    domain_id
                </p>
                <p>
                    Role_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    6.3
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Group-project
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    group_id
                </p>
                <p>
                    Project_id
                </p>
                <p>
                    Role_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    4. Project Admin
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    6.4
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Group-Domain
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    group_id
                </p>
                <p>
                    Project_id
                </p>
                <p>
                    Role_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    7
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Trust
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Trust_id
                </p>
                <p>
                    Trustor_user_id
                </p>
                <p>
                    Trustee_user_id
                </p>
                <p>
                    Impersonation
                </p>
                <p>
                    project
                </p>
                <p>
                    Deleted_at
                </p>
                <p>
                    Expire_at
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    C by (3) Owner – trustor
                </p>
                <p>
                    R by trustee (policy.json is empty)
                </p>
                <p>
                    UD by trustor , project admin, domain admin, cloud admin(policy.json is not compliant)
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    7.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Trust_role
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Trust_id
                </p>
                <p>
                    Role_id
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    C by (3) Owner – trustor
                </p>
                <p>
                    RUD by
                </p>
                <p>
                    (Empty)
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    8
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Token
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Token_id
                </p>
                <p>
                    Valid
                </p>
                <p>
                    Trust_id
                </p>
                <p>
                    Expires_at
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    C by
                </p>
                <p>
                    1) anonymous (authentication)
                </p>
                <p>
                    R by
                </p>
                <p>
                    7. Admin (v2)
                </p>
                <p>
                    13. service user
                </p>
                <p>
                    D by
                </p>
                <p>
                    3. Owner
                </p>
                <p>
                    5. (owning) Domain Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    8.1
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Revocation_list
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    [Token_id,
                </p>
                <p>
                    Expires_at]
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    R by
                </p>
                <p>
                    7. Admin (v2)
                </p>
                <p>
                    13. Service user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    9
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Credentials
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    10
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Service
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    11
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Endpoint
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    12
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Policy
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    CRUD by
                </p>
                <p>
                    6. Cloud Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
            </td>
            <td width="106" valign="top">
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    20
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    System
                </p>
            </td>
            <td width="284" colspan="5" valign="top">
                <p>
                    Assets related to configuration and system
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    21
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Audit data
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    8. Keystone System user
                </p>
                <p>
                    9. Other System user
                </p>
                <p>
                    10. System Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    22
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    PKI/SSL private key
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    8. Keystone System user
                </p>
                <p>
                    9. Other System user
                </p>
                <p>
                    10. System Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    23
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    PKI/SSL signing cert
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    All
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    24
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Availability of keystone service
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    25
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Access &amp; execute Query in DB
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    26
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Configuration parameter set
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    8. Keystone System user
                </p>
                <p>
                    9. Other System user
                </p>
                <p>
                    10. System Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    27
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    REMOTE_USER set
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    8. Keystone System user
                </p>
                <p>
                    12. External Identity provider user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    28
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    UUID generator
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    10. System Admin
                </p>
                <p>
                    12. Keystone DB user
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    30
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Crytpo libraries
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Encryption and Hashing Algoritms
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    10. System Admin
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    31
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Operation system
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    32
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Hosting Webserver
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    33
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Dependencies
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    34
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Persistence layer
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Only MySQL backend
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
                <p>
                    8. Keystone System user
                </p>
                <p>
                    10. System Admin
                </p>
                <p>
                    11. DB Admin
                </p>
                <p>
                    12. Keystone DB user
                </p>
            </td>
        </tr>
        <tr>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" valign="top">
            </td>
            <td width="142" colspan="3">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
            </td>
            <td width="390" colspan="6" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    50
                </p>
            </td>
            <td width="390" colspan="6" valign="top">
                <p>
                    Client
                </p>
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    51
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Dashboard
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    Dashboard to access Keystone service
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    52
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Keystone Client
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    53
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Browser
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
        <tr>
            <td width="36" valign="top">
                <p>
                    52
                </p>
            </td>
            <td width="106" valign="top">
                <p>
                    Login session
                </p>
            </td>
            <td width="147" colspan="3" valign="top">
                <p>
                    User login session to dashboard
                </p>
            </td>
            <td width="137" colspan="2" valign="top">
            </td>
        </tr>
    </tbody>
</table>
```
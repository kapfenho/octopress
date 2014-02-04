---
layout: post
title: "RBAC Security Model for Organizations"
date: 2012-12-28 12:43
comments: true
categories: 
---

Since several months we are working on designs and implementations of
identity management (IdM), a discipline increasing security and UX in the IT
landscape of organizations.

IdM is quite an interesting area, you need to touch and use a big set of
technologies and a bunch of already existing security standards. And you
need to talk to many people in an organization.

Here are some tips and rules you should follow, independent of products
you may use.

*Note: We are talking here about role based access control (RBAC), which is the
most prevalent authorization model. Other models can easily connect to
this design.*

## So let's start

No matter where your central point of structure or truth is located,
either an application or more common, a directory service, the design of
this area is the key.

A recent talk I did in a customer project:
[Directory Structure](http://agoracon.at/talks/directory-structure.html)

Implementing organizational hierarchies in IT systems usually gives you
headache and a lot of work, since those hierarchies tend to change
frequently in large organizations. But we don't like frequent changes of
our structure and security base. So we need a clever design here: allow
the data to change, not the structure.

In IT we always start with a layer model, even if we don't need
one - it sounds good in presentations :-)

## Our model or how we see this world

+ Layer one: identities (also called persons, users)
+ Layer two: organizational units (teams, departments, etc.)
+ Layer three: organizational roles (enterprise roles)
+ Layer four: application specific roles (groups)

Where the first one is a flat list of identities, the second one usually 
will be needed to implement a certain form of hierarchy and membership.

Talking in LDAP: use "member of" for memberships. It's easy to browse up 
the hierarchy. Optionally you may implement a second list "members"
for comfortably browsing down the hierarchy, in case the directory 
server does not automatically offer this. You should be aware this is 
data redundancy, but often very useful.

The third layer is a flat list without inheritance, each role
describing a certain general role in your organization. Keep this one
more business/organizational related and don't stick it to used
applications or products. 

The last one is a long, flat list of already existing, application
defined roles or groups. Those may be technical or business roles, 
it doesn't matter since the applications are the
master of those data. They need to maintain the mapping to their low-level
application permissions.

**What's an application?**

Well every kind of system we deal with is an application. It may be your
CRM system, an trouble-ticket system, an physical access control, and
your intranet system. And yes, your file server and mail server are
applications too.

## Wow, a lot of data here!

That's true, but we won't enter it manually. See below...

## So, how to use this?

Everything we do then is to create and maintain relations between
elements of those layers.

You won't map from layer one (identities) directly to layer four
(app-roles), this is like shooting yourself in the foot. Abstraction
means in our case to link via layer three, our organization roles.

## Please serve yourself

A self-service application may be used to create new relations or
end-date existing ones. In general that's a good idea, 
already medium sized organizations deal with a huge set of 
entries and relations.

Changes on those relations can be provisioned directly or as an 
approval workflow. You can even use both methods, depending on who
captures this change.

Request and approval permissions should be mapped again to
enterprise roles or in that special case to the owner of
the involved objects (in LDAP represented with the owner field). 

### An example

We decided to create a new organizational role for
*finance controllers*, since their responsibilities are already too 
distinct from *finance agents*, the role we used up to now. Our ERP 
team has already implemented a new ERP role *erp-controller*, covering 
their needs in a better way.

We will create this new organizational role *finance controller* and
then request to map this role to the ERP application role
*erp-controller*. This request shall be approved by the responsible
team, in this case the ERP security team.

We know this, because the organizational role *security-erp* is the owner
of application role *erp-controller* (like for all other ERP
application roles). Three persons are currently in *security-erp*, and we
hope not all of them are currently on vacation. An email has been sent
to notify them we need their approval.

I hope you get the point...

## Who is feeding this system?

### Identities and organizational units

Changes on persons and organizational groups (layer one and two) and
relations between those should come from the HR team. In case there 
is an HR system in place, an 
interface to this system is the way to go. 

Creation of new persons (new employees) and retirements are straight
forward to implement. Moving a person from one department to another
means deprovision the direct relations of the person and assign it to
the new team. Roles defined via the team will already be available to
the person, direct assignments to certain enterprise roles need to be
requested.

### Application roles

For large implementations an automatically exchange of app roles to the
individual applications is worth the effort. Usually the way from the
app to the IDM system is the more important one.

## How are those rules enforced?

Nowadays almost all applications support directory services by LDAP
integration. It's important to use this integration for authentication
and authorization.

### Authentication

Authentication can be done by an LDAP call (ldapbind, ldapcompare) or
offering single sign on (SSO). There are several options you can use for
SSO. A common way for access within an intranet 
(or using VPN) is to implement [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)). 

If you want to support access coming from Internet you need another
technology, e.g. own implementation with a common cookie, 
[OpenID](https://en.wikipedia.org/wiki/Openid), or other 
[central authentication services](https://en.wikipedia.org/wiki/Central_Authentication_Service).

### Authorization

Systems using the directory service will retrieve the application role 
the user has been assigned to.

For legacy systems not offering a directory integration you need to
provision this data (user and assigned app role). Data exchange in both
ways gives you the knowledge what rules are really in place (hacked into
the end system).

Huh, this post was getting longer than I thought.

Let me know your experience or opinion...




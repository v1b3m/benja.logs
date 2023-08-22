---
title: "Google OAuth 2.0"
categories:
  - dev
tags:
  - oauth
  - google
toc: true
---

How do we know if the user is using oauth or not?

## Logging in with Google

I've failed to enable OAuth login with Google, I keep getting stuck on the error screen below

{% include figure image_path="/assets/images/work/accounts.google.com_signin_oauth_error_authError=Cg9pbnZhbGlkX3JlcXVlc3QS3gEKWW91IGNhbid0IHNpZ24gaW4gdG8gdGhpcyBhcHAgYmVjYXVzZSBpdCBkb2Vzbid0IGNvbXBseSB3aXRoIEdvb2dsZSdzIE9BdXRoIDIuMCBwb2xpY3kgZm9yIGtlZXBpbmcgYXBwcyBzZWN1cmUuCgpZb3UgY2FuIGx.png" alt="this is a placeholder image" caption="" %}

### Following the provided links has also proven not to be very useful:

- [Learn more about this error](https://developers.google.com/identity/protocols/oauth2/policies#secure-response-handling)
- [error details](https://accounts.google.com/)

#### Learning more about the error

So, from the link, we learn that:

> Google may reject OAuth requests that don't originate from or resolve to a secure context.

##### Let's assume our OAuth requests are insecure! Where do we go from there?

We need to secure our OAuth requests

1. **Do our requests originate from a secure context**

   Our requests originate from a local server that is running over http. This may be the problem.

   If this is the problem, then we need to have our requests originating from an https(secure) server. This is a complex process and I don't want to go there.

   Can't we just determine the login type that a user is using within the app without having to log in with all the various options available. This should be attainable.

2. **Do our requests resolve to a secure context**

   Prolly!

### Do we need to have OAuth working for us to fix the bug?

Prolly not!

## TFA Management

In the admin settings, you can enable/disable 2FA.

{% include figure image_path="/assets/images/work/localhost_8080_admin_users_c8092fdb-7474-47ab-83b4-7934f7264c24(5.86).png" alt="this is a placeholder image" caption="http://localhost:8080/admin/users/c8092fdb-7474-47ab-83b4-7934f7264c24" %}

There's a bunch of utilities that handle TFA in `app/src/composables/use-tfa-setup.ts`

The `utils` are used by two files:

- `app/src/interfaces/_system/system-mfa-setup/system-mfa-setup.vue`
- `app/src/routes/tfa-setup.vue`

## Brainstorming

**NOTICE:** I'm not very good at Vue, so I gotta figure this shit out along the way.
{: .notice--primary}

I am trying to determine the setting change when oauth is used 

The google URI:

```
https://accounts.google.com/signin/oauth/error/v2?authError=Cg9pbnZhbGlkX3JlcXVlc3QS3gEKWW91IGNhbid0IHNpZ24gaW4gdG8gdGhpcyBhcHAgYmVjYXVzZSBpdCBkb2Vzbid0IGNvbXBseSB3aXRoIEdvb2dsZSdzIE9BdXRoIDIuMCBwb2xpY3kgZm9yIGtlZXBpbmcgYXBwcyBzZWN1cmUuCgpZb3UgY2FuIGxldCB0aGUgYXBwIGRldmVsb3BlciBrbm93IHRoYXQgdGhpcyBhcHAgZG9lc24ndCBjb21wbHkgd2l0aCBvbmUgb3IgbW9yZSBHb29nbGUgdmFsaWRhdGlvbiBydWxlcy4KICAaWWh0dHBzOi8vZGV2ZWxvcGVycy5nb29nbGUuY29tL2lkZW50aXR5L3Byb3RvY29scy9vYXV0aDIvcG9saWNpZXMjc2VjdXJlLXJlc3BvbnNlLWhhbmRsaW5nIJADKisKDHJlZGlyZWN0X3VyaRIbL2F1dGgvbG9naW4vZ29vZ2xlL2NhbGxiYWNr&client_id=25926454862-vgbe0fuo0hp125vjq59q6bmoqftf1rft.apps.googleusercontent.com
```

Log in with Google button
```
http://localhost:8080/auth/login/google?redirect=http%3A%2F%2Flocalhost%3A8080%2Fadmin%2Flogin%3Freason%3DSIGN_OUT%26continue%3D
```

Redirect: `http://localhost:8080/admin/login?reason=SIGN_OUT&continue=`

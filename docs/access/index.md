# Access and Accounting

Users at CSCS typically have one account that can be used to access all services at CSCS.

<div class="grid cards" markdown>

-   :fontawesome-solid-mountain-sun: __Multi Factor Authetification (MFA)__

    A guide to setting up and using MFA.

    [:octicons-arrow-right-24: Setting up and use MFA][mfa]

-   :fontawesome-solid-mountain-sun: __Getting Access__

    A project is required to get access to resources on Alps.
    Instructions on how to submit a project proposal is available on the main CSCS web site.

    [:octicons-arrow-right-24: Applying for an Alps project](https://www.cscs.ch/user-lab/applying-for-accounts)
</div>

There are different ways to authenticate your identity in order to access services at CSCS, using a password set by the user. Currently users can be authenticated with:

* Classic ssh with CSCS username/password, and also with ssh-keys
* CSCS username/password on different web services. Sessions can be independent from each other so, signing into one service does not necessarily sign the user into another one
* CSCS username/password on a Single Sign-On gate, which once the user is authenticated, can move between services (connected to this gate) without signing in again
* Username/password from an external institution, provided that his/her CSCS account has been "linked" to that external identity beforehand and the service uses the Single Sign-On gate

## Single Sign-On

Most services at CSCS are connected to the CSCS Single Sign-On gate.
This gives users the comfort of not having to sign in multiple times in each individual service connected to this gate and increases security.
Furthermore, the Single Sign-On gate allow users to recover their forgotten passwords and authenticate using a third-party account. The login page looks like

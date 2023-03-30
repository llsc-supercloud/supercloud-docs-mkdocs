Web Portal
==========

You can get to the Supercloud Web Portal at this URL:

<https://txe1-portal.mit.edu>

On this page you will find links to a number of useful tools. These
include

- [Page to add or remove ssh keys](#addingremoving-ssh-keys)
- [Home directory file browser](#browsing-your-home-directory)
- [Dynamic database launching system](databases.md)
- [Jupyter Notebook portal](jupyter-notebooks.md)

Portal Authentication
---------------------

There are three ways you can authenticate into the Web Portal: PKI
Certificate/Smart Card, MIT Touchstone/InCommon Federation, Username and
Password.

### MIT Touchstone/InCommon Federation

Most Supercloud users can log in via the MIT Touchstone/InCommon
Federation link. These include users with an active MIT Kerberos account
or account at most other educational institutions. When you click the
"MIT Touchstone/InCommon Federation" link for the first time you will
be taken to a page where you can select your institution. Note most
options are spelled out, for example "MIT" is listed as
"Massachusetts Institute of Technology". UMass Lowell users should
select "University of Massachusetts System". When you have selected
your institution, you should check the box that says "Remember my
Choice" and click select. You'll be taken to your institution's login
page where you can log in. Then you will be taken to the Portal main
page.

If you get a message saying "**The MIT Touchstone / InCommon Federation
principal you presented is not associated with an account on this
system. Please sign up at:
https://supercloud.mit.edu/requesting-account.**" and you already have
an account [contact us](http://supercloud.mit.edu/contact) and let us
know.

### Username and Password

If you do not have an active login for an educational institution you
may need to log into the portal using your username and password. Let us
know and we will let you know how to do this.

### PKI Certificate/Smart Card

Those who have a Smart Card or PKI Certificate can log in using the
first link on the page. If you would like to log in using your PKI
Certificate or Smart Card, please [let us
know](http://supercloud.mit.edu/contact).

Adding/Removing SSH Keys
------------------------

The first link on the Portal Home page is "/sshkeys/". Clicking on
this link takes you to a page where you can add new keys or remove old
ones. To add a new key, paste the key in the box at the bottom of the
page and click "Update Keys". You should see your new key populated in
the table and display similarly to the other keys listed. To remove an
old key, click the check box next to the key you would like to remove
and click "Update Keys" at the bottom. Instructions for generating a
new key are [here](../requesting-account.md#generating-ssh-keys).

Browsing your Home Directory
----------------------------

The second link on the Portal Home page is "/gridsan/". Click this
link and then select your username to see the files in your home
directory. You can click on a file to download it or click on a
directory to navigate to that directory.

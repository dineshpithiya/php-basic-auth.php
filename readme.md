## Overview
A client can authenticate to the Enterprise Gateway with a username and password combination using HTTP Basic Authentication. Whenever an HTTP Basic Authentication filter is configured, the Enterprise Gateway requests the client to present a username and password combination as part of the HTTP Basic challenge-response mechanism.

With HTTP Basic Authentication, the client's username and password are concatenated, base64-encoded, and passed in the Authorization HTTP header as follows:

```
Authorization: Basic dm9yZGVsOnZvcmRlbA==
```
## Configuration
The information specified on this screen informs the Enterprise Gateway where it can find user profiles for authentication purposes. The Enterprise Gateway can look up user profiles in the Enterprise Gateway's local repository, in a database, or in an LDAP directory. You can add Users to the local repository using the Users interface. See the Users tutorial for more information.

To configure the HTTP Basic Authentication filter, complete the following fields:

Name 
Enter a name for the filter here.

## Credential Format 
The username presented to the Enterprise Gateway during the HTTP Basic handshake can be of many formats, usually either username or Distinguished Name (DName). Because the Enterprise Gateway has no way of inherently telling one format from the other (for example, the client's username could be a DName), you must specify the format of the credential presented by the client. This format is then used internally by the Enterprise Gateway when performing authorization lookups against third-party Identity Management servers.

## Allow Client Challenge 
HTTP Basic Authentication can be configured with the following options:

## Direct Authentication 
The client sends up the Authorization HTTP Basic Authentication header in its first request to the server.
Challenge-Response Handshake 
The client does not send the Authorization header when sending its request to the server (it does not know that the server requires HTTP Basic Authentication). The server responds with an HTTP 401 response code, instructing the client to authenticate to the server by sending the Authorization header. The client then sends a second request, this time including the Authorization header and the relevant username and password.
The first case is used mainly for machine-to-machine transactions in which there is no human intervention. The second case is typical of situations where a browser is talking to a Web server. When the browser receives the HTTP 401 response to its initial request, it displays a dialog to enable the user to enter the username and password combination.

If you wish to force clients to always send the HTTP Basic Authorization header to the Enterprise Gateway, unselect the Allow client challenge checkbox. If you wish to allow clients to engage in the HTTP Basic Authentication challenge-response handshake with the Enterprise Gateway, ensure this feature is enabled by selecting this option.

## Remove HTTP Authentication Header 
Select this checkbox to remove the HTTP Authorization header from the downstream message. If this option is not selected, the incoming Authorization header is forwarded on to the destination Web Service.

## Repository Name 
This specifies the name of the Authentication Repository where all user profiles are stored. This can be in the Enterprise Gateway's local repository, in a database, or in an LDAP directory. Select a pre-configured Repository Name from the drop-down list.
```
<?php
function require_auth() {
	$AUTH_USER = 'admin';
	$AUTH_PASS = 'admin';
	header('Cache-Control: no-cache, must-revalidate, max-age=0');
	$has_supplied_credentials = !(empty($_SERVER['PHP_AUTH_USER']) && empty($_SERVER['PHP_AUTH_PW']));
	$is_not_authenticated = (
		!$has_supplied_credentials ||
		$_SERVER['PHP_AUTH_USER'] != $AUTH_USER ||
		$_SERVER['PHP_AUTH_PW']   != $AUTH_PASS
	);
	if ($is_not_authenticated) {
		header('HTTP/1.1 401 Authorization Required');
		header('WWW-Authenticate: Basic realm="Access denied"');
		exit;
	}
}
```
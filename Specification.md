# PGPAuth specification

1. Preamble
2. Versions
3. Requests and Actions
4. Signing
5. Transport
6. Notes

## 1. Preamble

PGPAuth is a system to send authenticated requests over untrusted networks.
It is a very simple system and easy to implement.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.



## 2. Versions

The PGPAuth specification is stored in a git repository on GitHub, you can reach it under the following URL:
https://github.com/PGPAuth/PGPAuth_Spec

Using the Releases (or Tags in standard git), you can read final versions of the specification, with the version-number in the name of the tag (or release).
Currently developed versions of the specification are refered to by their commit-hash, shortened to first 7 characters (standard in git).

Full version codes are either v<Tag-Name here> or v<commit-hash here>-git. You SHOULD use the Tag-version if it is a final version.

You SHOULD only make software implementing a final version of the PGPAuth specification. You MUST declare which version(s) are supported by your software.



## 3. Requests and Actions

Requests consists of the action, followed by a colon and the current timestamp, i.e. "open:1418777888". You can split a request by the colon to get the action ("open") and the timestamp.
The timestamp MUST be UTC, so users can issue requests to servers in another timezone. Server-software SHOULD give the administrator a way to set the maximum difference between the time on the server and the timestamp received by the user.

Actions are currently specified, there are only two:
- open
- close

The user-software MUST send the action in lowercase, the server-software SHOULD accept mixed-case actions (be strict in what you send but tolerant in what you receive).

Server-software MUST NOT accept mulitple request with the same action and exact same timestamp signed with the same key.

## 4. Signing

The Request is signed and transferred OpenPGP format. User-software MUST send the signed request with ASCII-Armor (not binary) and you SHOULD do a cleartext-signature (the plain request should be readable).
Server-software SHOULD accept binary-type signatures.

It is RECOMMENDED to disallow keys smaller than 2048 bit and user-software with key-generation SHOULD use 4096 bit as default key-length.
Sever-software MUST support RSA-signatures and SHOULD support as many OpenPGP-defined algorithms as possible.



## 5. Transport

The signed request is transferred with a HTTP POST-request. The signed request MUST be in the data-parameter. 
Client-software MUST support servers with HTTPS URLs. 




## 6. Notes (not part of the specification)

- The reference-implementation is PGPAuth_Android (https://github.com/PGPAuth/PGPAuth_Android). This is where it all started.
- When using HTTPS for the Server-URL, you should not use X.509-certificated with keys larger than 4096 bit, as some clients can't handle it (I'm looking at you, Apple) and it causes hard-to-debug errors.
- If you have questions, mail pgpauth@lf-net.org

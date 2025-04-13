Task 1A: HTTP Request Smuggling – Theoretical Analysis
How Do HTTP Request Smuggling Vulnerabilities Arise?
HTTP request smuggling vulnerabilities occur due to inconsistent parsing of HTTP request boundaries between front-end (e.g., proxies, load balancers) and back-end servers (e.g., Apache, NGINX). Specifically, the discrepancy is in how each server interprets the end of an HTTP request when both Content-Length and Transfer-Encoding headers are present.
In HTTP/1.1, these headers indicate the body length of a request. If the front-end and back-end servers interpret these headers differently, an attacker can smuggle hidden requests to the back end. This can result in:
•	Bypassing security controls
•	Session hijacking
•	Web cache poisoning
•	Request forgery and privilege escalation

Smuggling Techniques: CL.TE, TE.CL, and TE.TE
Technique	Description	Front-end Behavior	Back-end Behavior
CL.TE	Uses both Content-Length and Transfer-Encoding headers.	Trusts Content-Length	Trusts Transfer-Encoding
TE.CL	Reverses the trust direction.	Trusts Transfer-Encoding	Trusts Content-Length
TE.TE	Sends two Transfer-Encoding headers (e.g., one normal, one malformed).	Trusts first TE	Trusts second or ignores malformed one

Goal: Exploit differences to sneak an extra HTTP request through the front-end unnoticed by security controls.
In Which HTTP Version Is This Vulnerability Present?
•	HTTP/1.1 is vulnerable to HTTP request smuggling.
•	HTTP/2 and HTTP/3 are not affected, because:
o	They use binary framing instead of header-based parsing.
o	HTTP messages are encapsulated in discrete frames, eliminating ambiguity.
Why HTTP/1.1 Is Vulnerable
•	Supports persistent connections (multiple requests over one connection)
•	Relies on headers like Content-Length or Transfer-Encoding to determine request boundaries
•	Lacks a standardised way to resolve conflicting headers


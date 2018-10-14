# Project 7 - WordPress Pentesting

Time spent: 15 hours spent in total

## Pentesting Report
1. (Required) Authenticated Shortcode Tags Cross-Site Scripting (XSS)
  - [ ] Summary: This exploit takes advantage of WordPress's use of 'shortcode', a condensed verison of HTML used for parsing. In this case, we can write a line of HTML to 'fool' the system once it has been converted to shortcode. This attack is quite similiar to and SQL injection in that we are intentionally misleading the parser by feeding it malicious code that it believes to be valid, and thus executes.
    - Vulnerability types: XSS
    - Tested in version: 4.2
    - Fixed in version: 4.2.5
  - [ ] GIF Walkthrough: ![](https://github.com/ConnorCason/CodePath-Week-7-WordPress/blob/master/Vuln3.gif)
  - [ ] Steps to recreate: 
    - [ ] Sign into WordPress as an admin
    - [ ] Create a new post, using the text tab rather than the visual tab, and enter the payload: TEST!!![caption width="1" caption='<a href="' ">]</a>&lt;a href="http://onMouseOver='alert(1)'">Click me</a>
    - [ ] Before publishing, view the post preview and mouse over the message, which in this case in "Click me".
    - [ ] Mousing over this message executes potentially dangerous JS scripts
  - [ ] Affected source code: https://core.trac.wordpress.org/changeset/33499/branches/4.2

2. (Required) Unauthenticated Stored Cross-Site Scripting (CWE-79)
  - [ ] Summary: Exploit was centered around the fact that there was no character length limit when storing messages in the database. The deafult 64kB limit for SQL DB's truncates input beyond 64 kB resulting in malformed URL's which gives access to insert JavaScript without permission.
    - Vulnerability types: XSS
    - Tested in version: 4.2
    - Fixed in version: 4.2.1
  - [ ] GIF Walkthrough: ![](https://github.com/ConnorCason/CodePath-Week-7-WordPress/blob/master/Vuln_1.gif)
  - [ ] Steps to recreate: 
    - [ ] Sign into WordPress as an admin
    - [ ] Insert JS payload: &lt;a title='x onmouseover=alert(unescape(/hello%20world/.source)) style=position:absolute;left:0;top:0;width:5000px;height:5000px +++'></a> where '+++' indicates some sequence of characters that pushes the command to above 64kB
    - [ ] Publish Post
  - [ ] Affected source code: https://core.trac.wordpress.org/changeset/32307/branches/4.2
3. (Required) Authenticated Stored Cross-Site Scripting (CWE-79) (CVE: 2015-5622)
  - [ ] Summary: Similiar to the last Cross-Site Scripting attack, this one also relies on writing some clever HTML with embedded JS to gain access to the server side of WordPress. This time, the vulnerability was within the preview feature rather the after actual posting to the site.
    - Vulnerability types: XSS
    - Tested in version: 4.2 
    - Fixed in version: 4.2.3
  - [ ] GIF Walkthrough: ![](https://github.com/ConnorCason/CodePath-Week-7-WordPress/blob/master/Vuln_2.gif)
  - [ ] Steps to recreate: 
    - [ ] Sign into WordPress as an admin
    - [ ] Create a new post and fill the message body with the payload: &lt;a href="[caption code=">]</a>&lt;a title=" onmouseover=alert('test')  ">link&lt;/a>
    - [ ] Rather than publishing, preview the post
    - [ ] By hovering your mouse over the link (onmouseover) the payload is instructed to execute the JS script to compromise the system
  - [ ] Affected source code: https://core.trac.wordpress.org/log/branches/4.2?rev=33382&stop_rev=32430
  
  

## Assets

List any additional assets, such as scripts or files

## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)

GIFs created with [LiceCap](http://www.cockos.com/licecap/).

## Notes

Describe any challenges encountered while doing the work

## License

    Copyright [yyyy] [name of copyright owner]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
# Errors

<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>

The LOOP API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Malformed Request -- This should bring back a message, possibly indicating the missing data.
401 | Unauthorized -- Your token is invalid.
404 | Not Found -- The specified request could not be found.
418 | I'm a teapot.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

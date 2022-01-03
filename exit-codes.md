# exit codes

Exit codes are status codes returned by [[Unix]] programs upon exiting, indicating whether or not they terminated successfully or not.

[[POSIX]] and [[C]] both say that non-zero status codes indicate failure, while a 0 status code indicates success, but this is not always the case.

In [this article](https://www.jntrnr.com/exit-codes/), there&rsquo;s an example provided where `git log` returns a status of 128 per the POSIX standard, but incorrectly indicates that there was a failure.

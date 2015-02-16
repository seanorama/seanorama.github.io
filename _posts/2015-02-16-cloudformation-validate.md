---
layout: blog
published: false
---

Validating `CloudFormation` templates using `boto`.

This is the basics of what you need, after getting 'boto' to use your AWS credentials:

{% highlight python %}
template = # could be in the same file, read from elsewhere or generated
cfcon = boto.cloudformation.connect_to_region('us-west-2')
cfcon.validate_template(t.to_json())
{% endhighlight %}

Here is an example using Troposphere generated templates and compressing the file a bit.

{% highlight python %}
import sys
import boto
import boto.cloudformation
## ... snipped the troposphere imports

t = Template()
## ... snipped all the troposphere code

if __name__ == '__main__':
    template_compressed="\n".join([line.strip() for line in t.to_json().split("\n")])
    try:
        cfcon = boto.cloudformation.connect_to_region('us-west-2')
        cfcon.validate_template(template_compressed)
    except boto.exception.BotoServerError, e:
        sys.stderr.write("FATAL: CloudFormation Template Validation Error:\n%s\n" % e.message)
    else:
        sys.stderr.write("Successfully validated template!\n")
        print(template_compressed)
 {% endhighlight %}
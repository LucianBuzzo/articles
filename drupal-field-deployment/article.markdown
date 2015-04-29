How to painlessly deploy Drupal fields
======================================

Deploying new fields from a development environment onto a production server can
be a tricky and time consuming business in Drupal 7. A typical scenario might
look like this:
  - A client requests a new subtitle text field on the sites product nodes, "for better integration of cross-selling marketing
  solutions".
  - You create the field in your development environment (MAMP, XAMP, VM etc) by firing up Chrome and clicking though the GUI.
  - You check to make sure it all looks good, adding any additional styles or
    templating.
  - Happy with the changes, you transfer your new styles and template changes to
    the production server and then login and faithfully replicate creating the
    new subtitle text field on the live site.

Manually recreating fields on a production server is a tedious and error prone
business and something that I'd like to avoid entirely. This problem could be
solved with the use of the [Features][1] and [Strongarm][2] modules, but for me this isn't always
a viable option.  

The process of using Features and Strongarm to deploy updates
is not simple and requires you to split the functionality of your site into
*self-contained* parts of code and configuration, which can be both time
consuming and difficult, especially when working on a site that doesn't
use this method for deployment.  

My preferred approach in these situations is to
use a site deployment module. In short, a site deployment module is a custom
module that utilises a `.install` file to incrementally update a site. These
updates are run by implementing [hook_update_N()][3], where N is the modules
current update number. A basic .install file for a site deployment module looks
can be seen on the [dcycle][4] site (a fantastic resource for Drupal automation snippets).
If I were to create a new module called "mysite_deploy" containing this code:

    /**
     * Example update function that disables Overlay
     */
    function mysite_deploy_update_7001() {
      // some people don't like overlay...
      module_disable(array('overlay'));
    }

I could then push the module code to my production server, enable this module
through the GUI and then run update.php by clicking on the "update script" link
on the modules page. The function `mysite_deploy_update_7001` would then run and
the overlay module would be disabled.  

Using this method to create a new field is surprisingly tricky. Firstly you need
to create the new field using [field_info_field][5], passing
in a valid "field definition array". The you will need to create an instance for
each place that the new field is used using [field_create_instance][6], this
time using a "field instance definition array".  

For me, this is a daunting task. The information required to create a new field
is as confusing as it is diverse and becomes even more complicated when your
trying to deploy a non-standard field such as [entity reference][7].
I'm forever forgetting what "widget type" I need to be using and what
"cardinality" actually means. 

Luckily all this information is available through other Drupal API functions,
*if the field has already been created*. Using the two functions [field_info_field][8]
and [field_info_instance][9], I can add something like this to my themes
`template.php` and get all the information I need (as long as I can figure out what my entity type and bundle is).  

    $fieldName = 'field_fancy_marketing_text';
    $fieldInfo = field_info_field($fieldName);
    $bundle = 'product';
    $entityType = 'node';
    $fieldInstance = field_info_instance($entityType, $fieldName, $bundle);
    print_r($fieldInfo);
    print_r($fieldInstance);

The problem with this method is that it's particularly brutal ([print_r()][10] in
template.php?) and I still need to format the printed arrays into valid PHP.
Using [var_export][11] instead of print_r helps but it all still feels a bit
clunky. It's not fast and I'm having to edit unrelated files to get the
information I need.  

A better solution for me was to use a [Drush][12] script. If you haven't come 
across Drush before, it is a command line tool that can bootstrap and interface 
with drupal installations in various ways, from clearing caches to updating
modules. I highly recommend it, it's become an incredibly valuable part of my
drupal development toolkit.  
The script is run from the command line and pretty prints code that I can then
copy and paste into a `hook_update_N` function. A field name is provided as an
argument to the script, which then runs `field_info_field` and
`field_info_instance` to fetch the necessary field definition and instance
definition arrays. These arrays are then beautified and output to the command line
using the [heredoc style][13]. This script can be used 'as is' as long as it it's
run from inside a Drupal installation directory.  
I've posted the complete annotated script as a [bitbucket snippet][14]

I love this little drush script, but for me it was still bothersome that I had
to drop out of the `.install` file I was working on, run the script and copy and
paste the output. So I wrote a Vim plugin!

![Drupal field export vim plugin](fieldexport-plugin.gif)

Using this plugin I can type the machine name of a field, then go to normal mode 
and execute the function `DrupalExportField`. The word under the cursor is then
replaced with the PHP code generated by the drush script.



[1]: https://www.drupal.org/project/features
[2]: https://www.drupal.org/project/strongarm
[3]: https://api.drupal.org/api/drupal/modules%21system%21system.api.php/function/hook_update_N/7
[4]: http://dcycleproject.org/blog/65/basic-install-file-deployment-module
[5]: https://api.drupal.org/api/drupal/modules!field!field.crud.inc/function/field_create_field/7
[6]: https://api.drupal.org/api/drupal/modules%21field%21field.crud.inc/function/field_create_instance/7
[7]: https://www.drupal.org/project/entityreference
[8]: https://api.drupal.org/api/drupal/modules!field!field.info.inc/function/field_info_field/7
[9]: https://api.drupal.org/api/drupal/modules!field!field.info.inc/function/field_info_instance/7
[10]: http://php.net/manual/en/function.print-r.php
[11]: http://php.net/manual/en/function.var-export.php
[12]: http://www.drush.org/en/master/
[13]: https://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc
[14]: https://bitbucket.org/snippets/gravitywell_ltd/zEoR


# Administration

## Sign/Page Administration

### Develop New Page

Views for each Signage page are located in the [pages.py code](https://github.com/tjcsl/ion/blob/master/intranet/apps/signage/pages.py).  To add a new page, define the context in`pages.py`.   The template should be defined in `templates/signage/pages/<page_name>.html`.  

### Create New Page

Each Signage display renders multiple pages.  Each page is defined within a `Page` model.  Within Django admin, define the:

* name: Some descriptive name to represent the Page
* template: The location of the template \(`templates/signage/pages/<template>.html`\)
* function: The function defined in `pages.py` for server-side rendering of context \(e.g. `hello_world`\)
* button: The name of font-awesome icon to be used on the navigation bar \(e.g. `fa-bus`\)

The other fields can be left at their defaults.

Example:

### Create New Sign

Each Sign is defined within a `Sign` model.  Within Django admin, define the:

* name: Some friendly display name \(required\)
  * This is displayed on the main schedule page \(e.g. Curie Commons\)
* display: A unique name \(the Stick's hostname\) \(required\) \(e.g. cs-curie\)
* landscape: True if display is landscape and False if display is not landscape
* map\_location:
* img\_path: A URL to an image to display on the main schedule page.  Leave at default for the default TJ image
* lock\_page:
* display\_page:
* pages:

Example:

### Special Events

Signage is often used to display pages for special events that occur at TJ.  Generally, these are just iframes to some 

## Display Administration

### Rebooting

Often it is necessary to reboot Signage displays.  To perform a reboot manually, run  `sudo shutdown -r now` or  `sudo reboot`  within the Signage's terminal. To reboot the entire deployment, run `ansible  -i hosts -a "shutdown -r now" -u user -K signage`. 

### Connecting

To SSH to the Signage displays, a CSL VPN is necessary.  Ask a VPN admin to set up a certificate for you. 


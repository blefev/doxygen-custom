# doxygen-custom
Modified Dooxygen to match your organization.

# Customizing Doxygen
Doxygen provides a handful of ways to [customize the output](http://www.stack.nl/~dimitri/doxygen/manual/customize.html). The simplest way is to customize the HTML output.

Doxygen allows you to customize the HTML output by modifying a master HTML header, footer and stylesheet. You can then include additional stylesheets and javascript files. The following command will generate the default Doxygen files.

`doxygen -w html header.html footer.html customdoxygen.css`

# How to Integrate

To integrate this into your own project tell your doxyfile to use these 4 files in the HTML section (see the example site for an example of each file), or set these attributes in doxywizard.

* HTML_HEADER=header.html
* HTML_FOOTER=footer.html
* HTML\_EXTRA_STYLESHEET=customdoxygen.css
* HTML\_EXTRA_FILES=doxy-boot.js

# Matching Doxygen's Output to Your Organization
To match your Doxygen output with your organization, you will make heavy use of your browser's developer tools.

In this example, we will be matching the output to South Dakota School of Mine's branding. Branding information for SDSMT can be found [here](http://www.sdsmt.edu/Campus-Services/University-Relations-and-Media/Logos/). The branding guidelines detail the colors used and provide logos. The guidelines mention the logo should not be stretched or skewed, and all original parts must be intact. We can download a logo from the branding page, for example "White on Transparent" to use in our project. Note: Doxygen doesn't resize images, so we can resize it with a program like [imagemagick](https://www.imagemagick.org/script/index.php). To resize the logo using imagemagick to be 64px tall, without affecting the aspect ratio, run `convert -geometry x64 <image-in> <image-out>`.

Next, we need to do some digging with Web Inspector. In Chromium/Chrome/Firefox, the developer console can be brought up with `F12`, or the inspector with `Ctrl+Shift+C`.

Go to your organization's website, and pull up the inspector. Hover your mouse over the element you would like to see the style for and click it. Under the right hand side of the developer tools, you should see a list of CSS rules associated with the element. Crossed out rules generally mean that the rule was over-written by another. 

On SDSMT.edu, pull up the inspector and hover over one of the buttons on the navbar, for example "Apply". Go to Styles in Chromium/Chrome, or Rules in Firefox and you will see a rule for "a:active", with the color of the element defined as `color: #c1a868;`. This is the color we will later use for text on our Doxygen menubar. So store this color for future use.
We can repeat this process for other elements to discover what styles are being used. We can also select elements under the "Elements" tab under Chromium/Chrome, or the left main tab in Firefox dev tools. If we find the `container-fluid backgroundBlue` div under the `contentHeader` div, we will see a style class `backgroundBlue`. This contains the background color for the navbar, which we will also use in our menubar. Store it. We can also use this to look for a `font-family` rule, which will be useful for our project. Unfortunately, we can't use all of the fonts on the site because some of them are proprietary, paid-for fonts (e.g. Rockwell). If you are unsure that the rule selected corresponds to the element you are looking at, you can edit it in the developer tools, perhaps by changing the `color`. If it changes to the new color, your know it's the correct rule. We will use these steps a lot to figure out what rules need to be changed in Doxygen later.

An alternative way of finding the styles is looking directly at the stylesheets for the website. On SDSDMT.edu, press `Ctrl+U` or right click -> View Source to see the page source. We will see numerous stylesheets included for Bootstrap, Javascript Libraries, JQuery, and more. If we continue looking, we'll notice a stylesheet `/css/mines.css`. Click the link to follow it. This stylesheet contains most of the rules that define the custom colors and fonts of the site. 

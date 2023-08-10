# TYPO3 v12.4 Multi-Level Menu with MenuProcessor and Bootstrap v5.3.0

No additional CSS or JavaScript is needed for the responsive Multi-Level Menu. There is no mouse-over effect. The menu items are opened and closed via click.

TYPO3: Based on [Sitepackage Tutorial](https://docs.typo3.org/m/typo3/tutorial-sitepackage/master/en-us/) from Michael Schams and [Bootstrap Package](https://github.com/benjaminkott/bootstrap_package) from Benjamin Kott. Many Thanks!

Bootstrap: Based on [Bootstrap 5.3.0 Navbar Multi Level and Mega Menu Example](https://codepen.io/typo3-freelancer/pen/poEvyGj) of Simon Köhler. Many Thanks!
 
## Fluidtemplate with MenuProzessor
my_site_package/Configuration/TypoScript/setup.typoscript
```
page = PAGE
page {
     10 = FLUIDTEMPLATE
     10 {
        dataProcessing {
            10 = TYPO3\CMS\Frontend\DataProcessing\MenuProcessor
            10 {
                levels = 3
                includeSpacer = 1
                as = mainnavigation
                }
           }
        }
     }
```
## Main Navigation Partial (Bootstrap Navbar) with Partials for Subtitle (SubTitle) and Submenu (MainSub)
my_site_package/Resources/Private/Partials/Navigation/Main.html
```     
<html xmlns:f="http://typo3.org/ns/TYPO3/CMS/Fluid/ViewHelpers" data-namespace-typo3-fluid="true">
<nav class="navbar navbar-expand-lg">

    <a class="navbar-brand" href="#">Navbar</a>

    <button  class="navbar-toggler collapsed"
             type="button"
             data-bs-toggle="collapse"
             data-bs-target="#navbarNavDropdown"
             aria-controls="navbarNavDropdown"
             aria-expanded="false"
             aria-label="Toggle navigation"
    >
        <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="nav navbar-nav">
            <f:for each="{mainnavigation}" as="mainnavigationItem">
                <li class="nav-item{f:if(condition: mainnavigationItem.active, then: ' active')}{f:if(condition: mainnavigationItem.children, then:' dropdown')}">
                    <a class="nav-link{f:if(condition: mainnavigationItem.children, then:' dropdown-toggle')}"
                        href="{mainnavigationItem.link}"
                        target="{mainnavigationItem.target}"
                        <f:if condition="{mainnavigationItem.children}">
                            role="button"
                            data-bs-toggle="dropdown"
                            data-bs-auto-close="outside"
                            aria-expanded="false"
                        </f:if>
                        <f:render partial="Navigation/SubTitle" arguments="{menuSubtitle: mainnavigationItem}"/>
                    >
                        {mainnavigationItem.title}
                    </a>
                    <f:if condition="{mainnavigationItem.children}">
                        <f:render partial="Navigation/MainSub" arguments="{mainsubmenu: mainnavigationItem.children}"/>
                    </f:if>
                </li>
            </f:for>
        </ul>
    </div>
</nav>
</html>
```
## Submenu Partial (MainSub) with Partial for Subtitle (SubTitle)
my_site_package/Resources/Private/Partials/Navigation/MainSub.html
```
<ul class="dropdown-menu">
    <f:for each="{mainsubmenu}" as="subItem">
        <f:if condition="{subItem.spacer}">
            <f:then>
                <li class="dropdown-divider"></li>
            </f:then>
            <f:else>
                <li {f:if(condition: subItem.children, then: ' class="dropend"')}>
                <a class="dropdown-item{f:if(condition: subItem.active, then:' active')}{f:if(condition: subItem.children, then:' dropdown-toggle')}"
                    href="{subItem.link}"
                <f:if condition="{subItem.children}">
                    role="button"
                    data-bs-toggle="dropdown"
                    data-bs-auto-close="outside"
                    aria-expanded="false"
                </f:if>
                <f:render partial="Navigation/SubTitle" arguments="{menuSubtitle: subItem}"/>
                {f:if(condition: subItem.target, then: ' target="{subItem.target}"')}>
                <span class="dropdown-text">{subItem.title}
                            <f:if condition="{subItem.current}"><span class="visually-hidden">(current)</span></f:if>
                            </span>
                </a>
                <f:if condition="{subItem.children}">
                    <f:render partial="Navigation/MainSub" arguments="{mainsubmenu: subItem.children}"/>
                </f:if>
                </li>
            </f:else>
        </f:if>
    </f:for>
</ul>
```
## Partial for Subtitle (use Title or Subtitle for link title tag)
my_site_package/Resources/Private/Partials/Navigation/SubTitel.html
```
<f:if condition="{menuSubtitle.data.subtitle}">
<f:then>
    title="{menuSubtitle.data.subtitle}"
</f:then>
<f:else>
    title="{menuSubtitle.title}"
</f:else>
</f:if>
```
## Change Bootstrap Navbar Toggler Icon
The Toggler Icon is set in Bootstrap: _variables.scss
```
$navbar-light-toggler-icon-bg: url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 30 30'><path stroke='#{$navbar-light-icon-color}' stroke-linecap='round' stroke-miterlimit='10' stroke-width='2' d='M4 7h22M4 15h22M4 23h22'/></svg>") !default;
```
Change it through CSS (Example)
```
Color: stroke='rgba(51, 153, 255, 1)'
Thickness: stroke-width='3'

.navbar-toggler-icon {
  background-image: url("data:image/svg+xml;charset=utf8,%3Csvg viewBox='0 0 30 30' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath stroke='rgba(51, 153, 255, 1)' stroke-width='3' stroke-linecap='round' stroke-miterlimit='10' d='M4 7h22M4 15h22M4 23h22'/%3E%3C/svg%3E");
}
```

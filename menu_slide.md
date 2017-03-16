Notes: A part from the jquery plugins or big modules like superfish menu or nice menus, this is the simple jquery slide menu playing with parent menu links and sub menu links. Copy&paste this and change the block Id and class

change var subMenu_linksId and parentMenu_linksId.

/* Code start from here */

(function ($) {
    Drupal.behaviors.AnyName = {
        attach: function (context, settings) {
            // Set Parent menu and sub menu Id variable
            var subMenu_linksId = "block-menu-menu-about-course .menu li ul";
            var parentMenu_linksId = "block-menu-menu-about-course .menu li a";
            // Initially hide the sub menu.
            $('#' + subMenu_linksId).hide();
            // Set margin left to the sub menu, slightly right side from parent
            // menu ul.
            $('#' + subMenu_linksId).css({"margin-left": "7%"});
            // OnClick submenu to slide toggle.
            $('#' + parentMenu_linksId).click(function (e) {
                e.preventDefault();
                $(this).next('ul').slideToggle();
            });
        }
    };
})(jQuery);

# test7seasol.github.io

            
// Hello World


// Drawer open time root animation

            
              bind.root.addDrawerListener(object : DrawerLayout.DrawerListener {
                            override fun onDrawerSlide(drawerView: View, slideOffset: Float) {
                                // Adjust the X position smoothly as the drawer slides
                                bind.includeID.root.translationX = slideOffset * drawerView.width * 1f
                            }
            
                            override fun onDrawerOpened(drawerView: View) {
                                // Ensure it's properly positioned when fully opened
                                bind.includeID.root.translationX = drawerView.width * 1f
                            }
            
                            override fun onDrawerClosed(drawerView: View) {
                                // Move it back when closing
                                bind.includeID.root.translationX = 0f
                            }
            
                            override fun onDrawerStateChanged(newState: Int) {}
                        })
                        

    <style name="Base.Theme.GalleryEkta" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->

        <item name="android:statusBarColor">@android:color/white</item>
        <item name="android:windowLightStatusBar">true</item>
        <item name="android:windowOptOutEdgeToEdgeEnforcement">true</item>
        <item name="android:navigationBarColor">@color/white</item>
        <item name="android:windowLightNavigationBar">true</item>
        <item name="cardBackgroundColor">@color/white</item>
    </style>
    

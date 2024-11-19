# test7seasol.github.io

// Hello World

// statusbar hide & hide statusbar statusbar color
    
     <style name="Base.Theme.CompassApp" parent="Theme.Material3.Light.NoActionBar">
            <!-- Customize your light theme here. -->
            <!-- <item name="colorPrimary">@color/my_light_primary</item> -->
            <item name="android:statusBarColor">#FFFFFF</item>
            <item name="android:windowLightStatusBar">true</item>
            <item name="backgroundColor">#FFFFFF</item>
            <item name="android:backdropColor" tools:ignore="NewApi">#FFFFFF</item>
            <item name="android:background">#FFFFFF</item>
        </style>
    
        <style name="Theme.CompassApp" parent="Base.Theme.CompassApp" />


                applyStatusBarColor()
        
        private fun applyStatusBarColor() {
                val isDarkMode = AppCompatDelegate.getDefaultNightMode() == AppCompatDelegate.MODE_NIGHT_YES
        
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
                    // For Android 11 (API 30+) and above
                    val controller = window.insetsController
                    if (controller != null) {
                        if (isDarkMode) {
                            // White text/icons for dark mode
                            controller.setSystemBarsAppearance(
                                0, WindowInsetsController.APPEARANCE_LIGHT_STATUS_BARS
                            )
                        } else {
                            // Black text/icons for light mode
                            controller.setSystemBarsAppearance(
                                WindowInsetsController.APPEARANCE_LIGHT_STATUS_BARS,
                                WindowInsetsController.APPEARANCE_LIGHT_STATUS_BARS
                            )
                        }
                    }
                } else {
                    // For Android 6.0 (API 23) to Android 10 (API 29)
                    @Suppress("DEPRECATION") if (isDarkMode) {
                        window.decorView.systemUiVisibility = 0 // Clear light status bar flag
                    } else {
                        window.decorView.systemUiVisibility =
                            View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR // Add light status bar flag
                    }
                }
            }
    

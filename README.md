# test7seasol.github.io

       

            
// Hello World
        

// onbackPress extention updated onbackPress
       
          enableEdgeToEdge()
               binding = ActivityMainBinding.inflate(layoutInflater)
               setContentView(binding.root)
               window.statusBarColor = resources.getColor(android.R.color.white, theme)
       
               // Set light status bar icons (dark icons)
               WindowInsetsControllerCompat(window, window.decorView).isAppearanceLightStatusBars = true
       
               // Handle system insets
               ViewCompat.setOnApplyWindowInsetsListener(findViewById(android.R.id.content)) { v, insets ->
                   val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
                   v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
                   insets
               }
               
       
       fun AppCompatActivity.handleBackPress(onBackPressed: () -> Unit) {
           onBackPressedDispatcher.addCallback(this, object : OnBackPressedCallback(true) {
               override fun handleOnBackPressed() {
                   onBackPressed()
               }
           })
       }
       


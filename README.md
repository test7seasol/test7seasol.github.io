# test7seasol.github.io

       

            
// Hello World
        

// onbackPress extention updated onbackPress
       
       fun AppCompatActivity.handleBackPress(onBackPressed: () -> Unit) {
           onBackPressedDispatcher.addCallback(this, object : OnBackPressedCallback(true) {
               override fun handleOnBackPressed() {
                   onBackPressed()
               }
           })
       }
       


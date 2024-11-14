# test7seasol.github.io

// Hello World


// Ext Funcation
    
    inline fun <reified T : Activity> Context.startActivityWithBundle(bundle: Bundle? = null) {
        val intent = Intent(this, T::class.java).apply {
            bundle?.let { putExtras(it) }
        }
        startActivity(intent)
    }

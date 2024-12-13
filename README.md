# test7seasol.github.io

// Hello World

// Ai Image Genrater Api

    https://lexica.art/api/v1/search?q=Lifestyle Tools
    
    Api Chat BOT AI
    @POST
    Api ---> https://openai-cookbook-node.vercel.app/api/generate-answer
    Body x-www-from-urlencoded --> question: What is Android

    Api Chat BOT AI
    @POST
    http://keyword.saetatech.com/api/chat/word
    raw body: content: "what is android"


// Room Database room database demo

        val room_version = "2.6.1"
     implementation("androidx.room:room-runtime:$room_version")
        ksp("androidx.room:room-compiler:$room_version")
        implementation("androidx.room:room-ktx:$room_version")
        annotationProcessor("androidx.room:room-compiler:$room_version")

            plugins {
        alias(libs.plugins.android.application)
        alias(libs.plugins.kotlin.android)
        alias(libs.plugins.ksp)
    }
    
        plugins {
        alias(libs.plugins.android.application) apply false
        alias(libs.plugins.kotlin.android) apply false
        alias(libs.plugins.ksp).apply(false)
    }

    ksp = "1.9.10-1.0.13"
    ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }

    

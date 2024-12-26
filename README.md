# test7seasol.github.io

       
            
// Hello World
        
     
// DaggerHilt call api demo

    id("com.google.dagger.hilt.android") version "2.51.1" apply false

      id("kotlin-kapt")
    id("com.google.dagger.hilt.android")

     buildConfigField("String", "BASE_URL", "\"https://det/\"")
       
          buildFeatures {
               viewBinding = true
               buildConfig = true
           }
           
         //Dagger Hilt
           implementation("com.google.dagger:hilt-android:2.51.1")
           kapt("com.google.dagger:hilt-compiler:2.51.1")
       
           //Coroutine Scop
           implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.8.7")
           implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3")
           implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")
       
       @HiltAndroidApp
       class MyApplicationClass : Application() {
           override fun onCreate() {
               super.onCreate()
           }
       }

 
            @AndroidEntryPoint
       class DaggerHiltAct : BaseAct<ActivityDaggerHiltBinding>() {
       //    private val contactadapter = ContactAdapter()
           private val viewModel: YourViewModel by viewModels()
       
           override fun getActivityBinding(inflater: LayoutInflater) =
               ActivityDaggerHiltBinding.inflate(layoutInflater)
       
           override fun initUI() {
               lifecycleScope.launchWhenStarted {
                   viewModel.data.collectLatest { data ->
                       // Update your UI with the fetched data
       //                contactadapter.submitList(data)
                   }
               }
       
               lifecycleScope.launchWhenStarted {
                   viewModel.error.collectLatest { error ->
                       error?.let {
                           // Display a Toast or update an error view
                           ("Error: $it").log()
                           Toast.makeText(this@DaggerHiltAct, "Error: $it", Toast.LENGTH_SHORT).show()
                       }
                   }
               }
       
               viewModel.fetchData()
       
               bind.apply {
       //            rvid.adapter = contactadapter
               }
           }
       }

       //DaggerService.kt
              
       interface ApiService {
           @GET("endjson")
           suspend fun getYourData(): Response<ArrayList<ContactResItem>>
       }
       
       @Module
       @InstallIn(SingletonComponent::class)
       object NetworkModule {
       
           @Provides
           @Singleton
           fun provideRetrofit(): Retrofit {
               return Retrofit.Builder()
                   .baseUrl(BuildConfig.BASE_URL) // Using buildConfigField
                   .addConverterFactory(GsonConverterFactory.create())
                   .build()
           }
       
           @Provides
           @Singleton
           fun provideApiService(retrofit: Retrofit): ApiService {
               return retrofit.create(ApiService::class.java)
           }
       }

       //YourRepository.kt
              
       @Singleton
       class YourRepository @Inject constructor(private val apiService: ApiService) {
           suspend fun fetchData(): ArrayList<ContactResItem> {
               return apiService.getYourData() // Assuming your API returns a Response object
                   .body() ?: arrayListOf() // Return an empty list if response is null
           }
       }
       
       @HiltViewModel
       class YourViewModel @Inject constructor(private val repository: YourRepository) : ViewModel() {
           // StateFlow for holding data (non-nullable)
           private val _data = MutableStateFlow<ArrayList<ContactResItem>>(arrayListOf())
           val data: StateFlow<ArrayList<ContactResItem>> get() = _data
       
           // StateFlow for handling errors
           private val _error = MutableStateFlow<String?>(null)
           val error: StateFlow<String?> get() = _error
       
           fun fetchData() {
               viewModelScope.launch {
                   try {
                       val response = repository.fetchData() // Fetch data
                       _data.value = response // Update state with new data
                   } catch (e: Exception) {
                       _error.value = "Exception: ${e.message}" // Update state with error message
                   }
               }
           }
       }



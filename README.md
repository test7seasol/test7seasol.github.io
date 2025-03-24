# test7seasol.github.io

            
// Hello World


// IP Check free  my ipdata check my ip and location free ip check myipdata 
            
            @GET
            https://ipapi.co/json


// Proxy webview
    
                        
                        class MainActivity : AppCompatActivity() {
                        
                            private var binding: ActivityMain2Binding? = null
                        
                            // ✅ Mexico Proxy
                            private val proxyHost = "189.240.60.168"
                            private val proxyPort = 9090
                        
                         /* // ✅ Germany Proxy
                            private val proxyHost = "85.215.64.49"
                            private val proxyPort = 80*/
                        
                        
                            // ✅ URL to check if proxy works
                        //    private val testUrl = "https://ipapi.co/json"
                            private val testUrl = "https://29.mark.qureka.com/intro/question"
                        
                            override fun onCreate(savedInstanceState: Bundle?) {
                                super.onCreate(savedInstanceState)
                                enableEdgeToEdge()
                        
                                // Initialize binding
                                binding = ActivityMain2Binding.inflate(layoutInflater)
                                setContentView(binding?.root)
                        
                                setupWebView()
                                binding?.webView?.loadUrl(testUrl)
                            }
                        
                            private fun setupWebView() {
                                binding?.webView?.apply {
                                    settings.javaScriptEnabled = true
                                    settings.domStorageEnabled = true
                                    settings.useWideViewPort = true
                                    settings.loadWithOverviewMode = true
                                    settings.allowFileAccess = true
                        
                                    webViewClient = object : WebViewClient() {
                                        override fun shouldInterceptRequest(
                                            view: WebView?,
                                            request: WebResourceRequest?
                                        ): WebResourceResponse? {
                                            val url = request?.url.toString()
                                            logMessage("Intercepted request: $url")
                        
                                            return fetchViaProxy(url)
                                        }
                                    }
                        
                                    webChromeClient = WebChromeClient()
                                }
                            }
                        
                            private fun fetchViaProxy(url: String): WebResourceResponse? {
                                try {
                                    // ✅ Create OkHttpClient with Proxy
                                    val proxy = Proxy(Proxy.Type.HTTP, InetSocketAddress(proxyHost, proxyPort))
                                    val client = OkHttpClient.Builder().proxy(proxy).build()
                        
                                    // ✅ Make Request through Proxy
                                    val request = Request.Builder()
                                        .url(url)
                                        .header("User-Agent", "Mozilla/5.0 (Android Proxy WebView)")
                                        .build()
                        
                                    val response: Response = client.newCall(request).execute()
                        
                                    // ✅ Convert OkHttp response to WebResourceResponse
                                    val mimeType = response.header("Content-Type", "text/html") ?: "text/html"
                                    val encoding = response.header("Content-Encoding", "utf-8") ?: "utf-8"
                                    val inputStream = response.body?.byteStream()
                        
                                    logMessage("Proxy response: $mimeType | Encoding: $encoding")
                        
                                    return WebResourceResponse(mimeType, encoding, inputStream)
                                } catch (e: IOException) {
                                    logMessage("Proxy request failed: ${e.message}")
                                    return null
                                }
                            }
                        
                            private fun logMessage(msg: String) {
                                println("tag--->  $msg") // Log to console
                            }
                        
                            override fun onDestroy() {
                                super.onDestroy()
                                binding = null
                            }
                        }

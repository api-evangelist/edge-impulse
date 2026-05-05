---
title: "Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse"
url: "https://www.edgeimpulse.com/blog/gesture-controlled-smart-lights-with-a-galaxy-watch-and-edge-impulse/"
date: "Wed, 29 Apr 2026 18:26:04 GMT"
author: "Salman Hafeez"
feed_url: "https://www.edgeimpulse.com/blog/category/all/rss/"
---
<img alt="Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/Edge-AI-Watch-blogadaptivelarger.png" /><p>I clapped twice. The lights went off.</p><p>No voice command. No phone. No hub. Just a Galaxy Watch 4 on my wrist, an ML model running on its processor, and a UDP (User Datagram Protocol) packet fired across the local network.</p><p>I built it all myself, using Edge Impulse. Here is how it works.</p><figure class="kg-card kg-video-card kg-width-regular kg-card-hascaption">
            <div class="kg-video-container">
                <video height="580" poster="https://img.spacergif.org/v1/1378x580/0a/spacer.png" preload="metadata" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/media/2026/04/SmartWatchGestureControl_voiceover.mp4" width="1378"></video>
                <div class="kg-video-overlay">
                    <button class="kg-video-large-play-icon">
                        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                            <path d="M23.14 10.608 2.253.164A1.559 1.559 0 0 0 0 1.557v20.887a1.558 1.558 0 0 0 2.253 1.392L23.14 13.393a1.557 1.557 0 0 0 0-2.785Z">
                        </svg>
                    </button>
                </div>
                <div class="kg-video-player-container">
                    <div class="kg-video-player">
                        <button class="kg-video-play-icon">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M23.14 10.608 2.253.164A1.559 1.559 0 0 0 0 1.557v20.887a1.558 1.558 0 0 0 2.253 1.392L23.14 13.393a1.557 1.557 0 0 0 0-2.785Z">
                            </svg>
                        </button>
                        <button class="kg-video-pause-icon kg-video-hide">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <rect height="22" rx="1.5" ry="1.5" width="7" x="3" y="1">
                                <rect height="22" rx="1.5" ry="1.5" width="7" x="14" y="1">
                            </svg>
                        </button>
                        <span class="kg-video-current-time">0:00</span>
                        <div class="kg-video-time">
                            /<span class="kg-video-duration">1:58</span>
                        </div>
                        <input class="kg-video-seek-slider" max="100" type="range" value="0" />
                        <button class="kg-video-playback-rate">1&#xd7;</button>
                        <button class="kg-video-unmute-icon">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M15.189 2.021a9.728 9.728 0 0 0-7.924 4.85.249.249 0 0 1-.221.133H5.25a3 3 0 0 0-3 3v2a3 3 0 0 0 3 3h1.794a.249.249 0 0 1 .221.133 9.73 9.73 0 0 0 7.924 4.85h.06a1 1 0 0 0 1-1V3.02a1 1 0 0 0-1.06-.998Z">
                            </svg>
                        </button>
                        <button class="kg-video-mute-icon kg-video-hide">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M16.177 4.3a.248.248 0 0 0 .073-.176v-1.1a1 1 0 0 0-1.061-1 9.728 9.728 0 0 0-7.924 4.85.249.249 0 0 1-.221.133H5.25a3 3 0 0 0-3 3v2a3 3 0 0 0 3 3h.114a.251.251 0 0 0 .177-.073ZM23.707 1.706A1 1 0 0 0 22.293.292l-22 22a1 1 0 0 0 0 1.414l.009.009a1 1 0 0 0 1.405-.009l6.63-6.631A.251.251 0 0 1 8.515 17a.245.245 0 0 1 .177.075 10.081 10.081 0 0 0 6.5 2.92 1 1 0 0 0 1.061-1V9.266a.247.247 0 0 1 .073-.176Z">
                            </svg>
                        </button>
                        <input class="kg-video-volume-slider" max="100" type="range" value="100" />
                    </div>
                </div>
            </div>
            <figcaption><p><span style="white-space: pre-wrap;">Gesture-controlled smart lights demo using Edge Impulse and a Galaxy watch</span></p></figcaption>
        </figure><h2 id="the-idea">The idea</h2><p>Smartwatches are underrated as gesture remotes. They sit on your wrist, they have an IMU, and &#x2014; crucially &#x2014; they are already there when you want to adjust the lights without hunting for your phone. The catch is that getting a custom ML model onto a Wear OS watch is supposed to be painful: ABI mismatches, stripped-down runtimes, JNI boilerplate, toolchains that argue with each other.</p><p>Two things in particular are worth calling out:</p><p><strong>Capturing training data from a wearable is normally a project in itself.</strong> The watch does not expose a USB data port you can plug into a laptop. You have to build a companion app, handle Bluetooth or Wi-Fi transport, and somehow get the samples into whatever format your training pipeline expects. Here, I built a small forwarding app that streams IMU data directly to the Edge Impulse ingestion API &#x2014; so the watch becomes a first-class data source without any intermediate tooling.</p><p><strong>Deploying to a wearable is where most TFLite-based projects stall.</strong> Pre-built Android TFLite libraries ship <code>arm64-v8a</code> binaries, but the Galaxy Watch 4 runs a 32-bit <code>armeabi-v7a</code> userspace despite its 64-bit processor. Edge Impulse&#x2019;s C++ Android deployment sidesteps the problem entirely by exporting TFLite Micro source code alongside the model. CMake compiles everything from scratch targeting whatever ABI you specify. One line in <code>build.gradle.kts</code>:</p><pre><code class="language-kotlin">ndk { abiFilters.add(&quot;armeabi-v7a&quot;) }
</code></pre><p>That is the entire ABI configuration. Everything else is handled by the build.</p><h2 id="collecting-data">Collecting data</h2><p>The Galaxy Watch 4 exposes accelerometer and gyroscope data via the standard Android <code>SensorManager</code> API at up to 50 Hz. I built a small data-forwarding app that streamed both sensors over UDP to the <a href="https://docs.edgeimpulse.com/apis/ingestion">Edge Impulse data ingestion endpoint</a> &#x2014; six channels in total: <code>acc_x</code>, <code>acc_y</code>, <code>acc_z</code>, <code>gyr_x</code>, <code>gyr_y</code>, <code>gyr_z</code>.</p><p>I recorded three gesture classes:</p><ul><li><strong>double_clap</strong> &#x2014; two sharp claps</li><li><strong>color_cycle</strong> &#x2014; two quick wrist twists</li><li><strong>idle</strong> &#x2014; wrist at rest, normal movement</li></ul><p>About 30 samples per class, two seconds per sample, with each sample containing a single gesture performed once within that window. The whole collection session took roughly 20 minutes.</p><p>I created a dedicated application to collect sensor data from the watch and upload it directly to my Edge Impulse project using the project API key. Here is how it works:</p><figure class="kg-card kg-video-card kg-width-regular kg-card-hascaption">
            <div class="kg-video-container">
                <video height="1310" poster="https://img.spacergif.org/v1/3358x1310/0a/spacer.png" preload="metadata" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/media/2026/04/SmartWatchDataCollector.mp4" width="3358"></video>
                <div class="kg-video-overlay">
                    <button class="kg-video-large-play-icon">
                        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                            <path d="M23.14 10.608 2.253.164A1.559 1.559 0 0 0 0 1.557v20.887a1.558 1.558 0 0 0 2.253 1.392L23.14 13.393a1.557 1.557 0 0 0 0-2.785Z">
                        </svg>
                    </button>
                </div>
                <div class="kg-video-player-container">
                    <div class="kg-video-player">
                        <button class="kg-video-play-icon">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M23.14 10.608 2.253.164A1.559 1.559 0 0 0 0 1.557v20.887a1.558 1.558 0 0 0 2.253 1.392L23.14 13.393a1.557 1.557 0 0 0 0-2.785Z">
                            </svg>
                        </button>
                        <button class="kg-video-pause-icon kg-video-hide">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <rect height="22" rx="1.5" ry="1.5" width="7" x="3" y="1">
                                <rect height="22" rx="1.5" ry="1.5" width="7" x="14" y="1">
                            </svg>
                        </button>
                        <span class="kg-video-current-time">0:00</span>
                        <div class="kg-video-time">
                            /<span class="kg-video-duration">1:22</span>
                        </div>
                        <input class="kg-video-seek-slider" max="100" type="range" value="0" />
                        <button class="kg-video-playback-rate">1&#xd7;</button>
                        <button class="kg-video-unmute-icon">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M15.189 2.021a9.728 9.728 0 0 0-7.924 4.85.249.249 0 0 1-.221.133H5.25a3 3 0 0 0-3 3v2a3 3 0 0 0 3 3h1.794a.249.249 0 0 1 .221.133 9.73 9.73 0 0 0 7.924 4.85h.06a1 1 0 0 0 1-1V3.02a1 1 0 0 0-1.06-.998Z">
                            </svg>
                        </button>
                        <button class="kg-video-mute-icon kg-video-hide">
                            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path d="M16.177 4.3a.248.248 0 0 0 .073-.176v-1.1a1 1 0 0 0-1.061-1 9.728 9.728 0 0 0-7.924 4.85.249.249 0 0 1-.221.133H5.25a3 3 0 0 0-3 3v2a3 3 0 0 0 3 3h.114a.251.251 0 0 0 .177-.073ZM23.707 1.706A1 1 0 0 0 22.293.292l-22 22a1 1 0 0 0 0 1.414l.009.009a1 1 0 0 0 1.405-.009l6.63-6.631A.251.251 0 0 1 8.515 17a.245.245 0 0 1 .177.075 10.081 10.081 0 0 0 6.5 2.92 1 1 0 0 0 1.061-1V9.266a.247.247 0 0 1 .073-.176Z">
                            </svg>
                        </button>
                        <input class="kg-video-volume-slider" max="100" type="range" value="100" />
                    </div>
                </div>
            </div>
            <figcaption><p><span style="white-space: pre-wrap;">Data collection demo using Edge Impulse and a Galaxy watch</span></p></figcaption>
        </figure><p>You can download the app source from the <a href="https://github.com/edgeimpulse/smartwatch_data_collector" rel="noreferrer">smartwatch_data_collector repository</a> on GitHub and follow the instructions in the README to add your own project&#x2019;s API key using the <code>adb</code> command.</p><p>After collection, samples were reviewed and cleaned using Edge Impulse Studio&#x2019;s sample editor &#x2014; this involved trimming samples where the gesture started too late or was cut off at the edge of the window, and removing any samples with obvious sensor dropout.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse" class="kg-image" height="1112" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/Screenshot-2026-03-26-at-01.45.52.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">Gesture data sample example</span></figcaption></figure><hr /><h2 id="configuring-the-impulse">Configuring the impulse</h2><p>The impulse uses IMU sensor data &#x2014; accelerometer and gyroscope &#x2014; as input, with a 2-second window at 50 Hz giving 100 data points per channel. A spectral analysis processing block extracts frequency-domain features from each axis, which are then fed into a small dense neural network classifier.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse" class="kg-image" height="982" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/Screenshot-2026-03-30-at-11.17.39.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">Impulse configuration in Studio</span></figcaption></figure><h2 id="extracting-features">Extracting features</h2><p>With some fine-tuning of the spectral analysis block parameters, the features for <code>double_clap</code>, <code>color_cycle</code>, and <code>idle</code> became clearly separable in the feature explorer.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse" class="kg-image" height="1088" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/Screenshot-2026-03-30-at-11.19.49.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">Generated features overview in Studio</span></figcaption></figure><h2 id="training">Training</h2><p>Training is straightforward inside Edge Impulse Studio. After training, I selected the int8 post-training quantized variant to keep the model compact and suitable for on-device inference. Final accuracy on the test set came out at <strong>92%</strong>, with most of the misses being due to the limited number of samples in the training dataset.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Gesture-Controlled Smart Lights with a Galaxy Watch and Edge Impulse" class="kg-image" height="860" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/Screenshot-2026-04-23-at-23.37.22.png" width="1378" /><figcaption><span style="white-space: pre-wrap;">Training validation accuracy and confusion matrix in Studio</span></figcaption></figure><h2 id="wiring-it-up">Wiring it up</h2><p>The inference pipeline in the watch app runs in two stages.</p><p>First, a motion detector monitors the accelerometer variance over a rolling 10-sample window at 50 ms intervals. At rest, variance sits near zero. A gesture spikes it above a threshold almost immediately &#x2014; no model needed for this step, just arithmetic. This keeps the CPU nearly idle when nothing is happening.</p><p>When motion is detected, the app waits one second to let the gesture fully develop, then passes the last two seconds of IMU data to the Edge Impulse classifier via a JNI call:</p><pre><code class="language-kotlin">val result     = runInference(sensorCollector.buildInputArray(featureCount))
val label      = result?.substringBefore(&quot;:&quot;)
val confidence = result?.substringAfter(&quot;:&quot;)?.toFloatOrNull() ?: 0f
</code></pre><p>Results below 65% confidence are discarded. Everything else maps to a smart light command &#x2014; toggle on/off or color cycle &#x2014; sent as a UDP packet to the bulb&#x2019;s local IP.</p><p>A partial wake lock keeps the sensor pipeline alive when the screen is off, which is the normal state for a watch.</p><h2 id="results">Results</h2><p>End-to-end latency from gesture completion to bulb response is around 1.1 seconds &#x2014; the one-second capture window dominates. That is perfectly comfortable for light control; you are not trying to play a video game.</p><p>The model runs reliably across normal daily movement. The variance gate filters out walking and typing almost completely, so false triggers are rare in practice.</p><h2 id="what%E2%80%99s-next">What&#x2019;s next</h2><p>The obvious improvement is more robust training data &#x2014; more samples, more variation in wrist angle and movement speed. Beyond that, the same architecture would support scene presets (a slow double-tap for &#x201c;movie mode&#x201d;, a Z-motion for &#x201c;goodnight&#x201d;), which would make this genuinely useful rather than just a satisfying demo. Furthermore, more devices can be added to demonstrate extended functionalities within a smart home setting.</p><p>Want to go deeper? The full source code, including the Edge Impulse deployment, the Wear OS app, and a helper script to swap in a retrained model in one command, is on GitHub in the <a href="https://github.com/edgeimpulse/smartwatch_gesture_control" rel="noreferrer">smartwatch_gesture_control repository</a>.</p>

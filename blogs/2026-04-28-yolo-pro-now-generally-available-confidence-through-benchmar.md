---
title: "YOLO-Pro Now Generally Available: Confidence Through Benchmarking"
url: "https://www.edgeimpulse.com/blog/yolo-pro-now-generally-available-confidence-through-benchmarking/"
date: "Tue, 28 Apr 2026 06:00:03 GMT"
author: "Brian McFadden"
feed_url: "https://www.edgeimpulse.com/blog/category/all/rss/"
---
<img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/YOLO-Pro-Benchmarking-Blog--4-.png" /><p>When our research team first took on the challenge of developing YOLO-Pro, the goal was clear: to build a modern family of object detection architectures that deliver state-of-the-art performance on edge devices, under real constraints, with real-world latency, memory, and power limits, backed by permissive licensing.</p><p>Since our <a href="https://www.edgeimpulse.com/blog/introducing-yolo-pro-object-detection-optimized-for-the-edge/">initial announcement post</a>, YOLO&#x2011;Pro has evolved from a preview to a production-proven model family. Along the way, it&#x2019;s been iterated on, stress-tested, benchmarked, and deployed across a wide range of datasets, model sizes, and hardware targets. And today, we&#x2019;re excited to announce that YOLO&#x2011;Pro is officially generally available. &#x1f64c;</p><p>But moving from developer preview to general availability doesn&#x2019;t come from optimism; it comes from evidence.</p><p>In this post, we&#x2019;ll walk through the benchmark results that gave the team confidence to make that call. We&#x2019;ll break down how YOLO&#x2011;Pro compares against widely used YOLO families like YOLOv11 and YOLOv5, evaluate both float32 and int8 performance, and examine how these models behave across multiple sizes and datasets.</p><p>Let&#x2019;s dig into the data.</p><h2 id="a-quick-recap">A quick recap</h2><p>Traditional YOLO models are powerful, but they were designed with cloud environments and academic datasets like COCO in mind, with restrictive licensing for commercial use. </p><p>YOLO-Pro flips that paradigm. Edge Impulse engineered these models from the ground up to excel in real-word edge deployments. That means better performance, lower latency, and smarter resource usage for your embedded object detection projects, all available as a part of the Edge Impulse platform and governed by our terms of use.</p><h2 id="models-compared">Models compared</h2><p>To compare performance, we assessed YOLO-Pro, YOLOv11, and YOLOv5 models at four different sizes: nano, small, medium, and large. We used YOLOv11 and YOLOv5 as they are the two most frequently used YOLO models within the Edge Impulse developer community. </p><p><strong>Model size parameter counts:</strong></p>
<!--kg-card-begin: html-->
<table class="custom-table">
  <caption>Parameter counts for model sizes</caption>
  <thead>
    <tr>
      <th></th>
      <th>nano</th>
      <th>small</th>
      <th>medium</th>
      <th>large</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>YOLO-Pro</td>
      <td>2.3 M</td>
      <td>7.2 M</td>
      <td>17.8 M</td>
      <td>32.6 M</td>
    </tr>
    <tr>
      <td>YOLOv11</td>
      <td>2.6 M</td>
      <td>9.4 M</td>
      <td>20.0 M</td>
      <td>25.2 M</td>
    </tr>
    <tr>
      <td>YOLOv5</td>
      <td>1.8 M</td>
      <td>7.0 M</td>
      <td>20.8 M</td>
      <td>46.1 M</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<h2 id="results-summary">Results summary</h2><p>We compared YOLO-Pro performance against YOLOv11 and YOLOv5 in two ways: using <a href="#benchmark-one-selected-datasets" rel="noreferrer">eight specifically selected datasets</a> and <a href="#benchmark-two-real-world-projects" rel="noreferrer">ninety-five sampled real-world projects</a>. See the respective sections of this post for the methodologies and detailed results. </p><p>The benchmarking showed that YOLO&#x2011;Pro is a strong alternative to YOLOv11 and YOLOv5, typically delivering better performance than both model families at int8 precision, while achieving performance comparable to YOLOv11, and often exceeding YOLOv5, at float32 precision.</p><p>Across a wide range of real&#x2011;world conditions, YOLO&#x2011;Pro demonstrates robust int8 performance, which is critical for fast, efficient inference on embedded hardware. It excels in fixed&#x2011;camera deployments with small and dense bounding boxes, variable backgrounds, mixed color and grayscale inputs, partial occlusion, and changing perspectives, supporting both industrial vision and augmented reality use cases.</p><p>We&#x2019;re happy to have a strong baseline to build upon. The story does not end here though. Check out the <a href="#what%E2%80%99s-next-for-yolo-pro" rel="noreferrer">What&#x2019;s next for YOLO-Pro</a> section at the end of this post to see what&#x2019;s coming.</p><p>Notable results across the eight datasets selected for benchmarking:</p><ul><li>YOLO-Pro far out performs YOLOv11 at int8 precision, largely due to an insurmountable amount of error introduced through post training quantization of the YOLOv11 models (a known issue with YOLO models since YOLOv8, and something that we addressed in the development of YOLO-Pro)</li><li>YOLO-Pro had better results compared to YOLOv5 at all four model sizes tested with int8 precision</li></ul><p>On a sample of ninety-five real-world projects, YOLO-Pro consistently outperforms YOLOv11 and YOLOv5 at int8 precision:</p>
<!--kg-card-begin: html-->
<table class="custom-table">
  <caption>YOLO-Pro vs YOLOv11 performance across 95 projects</caption>
  <thead>
    <tr>
      <th></th>
      <th colspan="2">int8</th>
      <th colspan="2">float32</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Model</td>
      <td><strong>YOLO-Pro</strong></td>
      <td>YOLOv11</td>
      <td>YOLO-Pro</td>
      <td><strong>YOLOv11</strong></td>
    </tr>
    <tr>
      <td>Top performing project count</td>
      <td><strong>70</strong></td>
      <td>25</td>
      <td>37</td>
      <td><strong>58</strong></td>
    </tr>
    <tr>
      <td>Top performing project percentage</td>
      <td><strong>74%</strong></td>
      <td>26%</td>
      <td>39%</td>
      <td><strong>61%</strong></td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->

<!--kg-card-begin: html-->
<table class="custom-table">
  <caption>YOLO-Pro vs YOLOv5 performance across 95 projects</caption>
  <thead>
    <tr>
      <th></th>
      <th colspan="2">int8</th>
      <th colspan="2">float32</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Model</td>
      <td><strong>YOLO-Pro</strong></td>
      <td>YOLOv11</td>
      <td><strong>YOLO-Pro</strong></td>
      <td>YOLOv11</td>
    </tr>
    <tr>
      <td>Top performing project count</td>
      <td><strong>89</strong></td>
      <td>6</td>
      <td><strong>78</strong></td>
      <td>7</td>
    </tr>
    <tr>
      <td>Top performing project percentage</td>
      <td><strong>94%</strong></td>
      <td>6%</td>
      <td><strong>82%</strong></td>
      <td>18%</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<h2 id="benchmark-one-selected-datasets">Benchmark one: selected datasets</h2><p>We assessed the performance of YOLO-Pro across eight permissibly licensed datasets that captured a variety of scenarios and potential applications. These datasets were chosen for their representative aspects that one may come across in industrial tasks: fixed camera positions, variable backgrounds, different object orientations and sizes, partial occlusion, and other factors. </p><h3 id="methodology">Methodology</h3><p>YOLO-Pro, YOLOv11, and YOLOv5 models were trained once against each of the selected  dataset. The caveat to training once is that, in the case when YOLOv11 exhibited catastrophic failure for int8 model precision, it was retrained to ensure the failures were indeed consistent behavior.</p><p>The training run for each model consisted of 100 epochs and had input image resolution matched for each comparison (320x320x3 or 640x640x3). The performance of each model, assessed by mAP@[.5:.95] (mean average precision over IoU thresholds from 0.5 to&#xa0;0.95) scores on the test samples, was calculated using float32 and int8 precision TensorFlow Lite models within Edge Impulse.</p><p>Note that the default YOLO-Pro architecture option is <code>No Attention with ReLU</code>  because it has the widest hardware compatibility. However, <code>Attention with SiLU</code> was selected for benchmarking to match the SiLU activation function in YOLOv11 and YOLOv5.</p><h3 id="analysis">Analysis</h3><p>An analysis for each dataset can be found in the sections to follow. Before diving into individual datasets, it&#x2019;s worth clarifying how to read the results that follow. For each dataset, we report both float32 and int8 precision. The float32 numbers provide a useful reference for raw model capability, but the analysis gets more interesting when looking at int8 results, as these most closely reflect how the models are deployed in practice on edge devices. </p><p><strong>Datasets:</strong></p><ol><li><a href="#bottle-crate-dataset" rel="noreferrer">Bottle crate</a> </li><li><a href="#cat-vs-dog-dataset" rel="noreferrer">Cat vs dog</a></li><li><a href="#parking-lot-occupancy-dataset" rel="noreferrer">Parking lot occupancy</a></li><li><a href="#pcbs-on-a-conveyor-belt-dataset" rel="noreferrer">PCBs on a conveyor belt</a></li><li><a href="#trail-camera-dataset" rel="noreferrer">Trail camera</a></li><li><a href="#trash-bin-in-urban-settings-dataset" rel="noreferrer">Trash bin in urban settings</a></li><li><a href="#washers-and-bolts-dataset" rel="noreferrer">Washers and bolts</a></li><li><a href="#worker-assistant-dataset" rel="noreferrer">Worker assistant</a></li></ol><hr /><h3 id="bottle-crate-dataset">Bottle crate dataset</h3><p><strong>Takeaway:</strong> YOLO-Pro is a great choice of int8 model for fixed camera position and consistently sized small bounding box applications.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the bottle crate dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of a bottle crate, seen from above, in various orientations with bottles that have their lids on and off </td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>2 (empty, full)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>97</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>33</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1238" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-bottles-rack.png" width="1668" /><figcaption><span style="white-space: pre-wrap;">Image examples from the bottle crate dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/image.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the bottle crate dataset</span></figcaption></figure><p><strong>float32:</strong> YOLOv11 consistently led mAP across all model sizes, with YOLO&#x2011;Pro tracking closely behind and YOLOv5 trailing throughout.</p><p><strong>int8:</strong> Quantization shifted the balance. YOLO&#x2011;Pro emerged as the strongest option at the nano, medium, and large sizes, while YOLOv11 retained a narrow lead only at the small size. YOLOv5 improved relative placement as model size increased.</p><hr /><h3 id="cat-vs-dog-dataset">Cat vs dog dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro performs well on this commonly used dataset where there are highly divergent backgrounds and many different sized bounding boxes that appear all over the images.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the cat vs dog dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of cats and dogs with their faces labelled in various indoor and outdoor settings </td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>2 (cat, dog)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>2,323</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>590</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1394" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-cat-vs-dog.png" width="1682" /><figcaption><span style="white-space: pre-wrap;">Image examples from the cat vs dog dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/cat_vs_dog_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the cat vs dog dataset</span></figcaption></figure><p><strong>float32:</strong> YOLOv11 took the top spot across all model sizes, while second place alternated with YOLO&#x2011;Pro leading at the smaller sizes and YOLOv5 at the larger variants.</p><p><strong>int8:</strong> Post&#x2011;quantization, YOLOv11 remained competitive at nano and small but suffered severe degradation at medium and large. YOLO&#x2011;Pro held a clear advantage over YOLOv5 across all sizes and remained close to YOLOv11 where the latter held up.</p><hr /><h3 id="parking-lot-occupancy-dataset">Parking lot occupancy dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro can perform well with datasets that have a large number of bounding boxes in images. This result also shows another fixed camera example where YOLO-Pro performs well. </p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the parking lot occupancy dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of cars in a parking lot taken from a fixed place camera above the lot</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>1 (car)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>3,594</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>879</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1144" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-parking-lot-occupancy.png" width="1686" /><figcaption><span style="white-space: pre-wrap;">Image examples from the parking lot occupancy dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/parking_lot_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the parking lot occupancy dataset</span></figcaption></figure><p><strong>float32:</strong> YOLO&#x2011;Pro led at the nano and small sizes, while YOLOv11 overtook it at the medium and large scales. YOLOv5 consistently ranked third.</p><p><strong>int8:</strong> After quantization, YOLO&#x2011;Pro delivered the most stable performance across all four sizes. YOLOv11 and YOLOv5 varied more widely, with YOLOv11 exhibiting a pronounced failure at the large size.</p><hr /><h3 id="pcbs-on-a-conveyor-belt-dataset">PCBs on a conveyor belt dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro is a good choice for fixed camera conveyor belt style applications even when the bounding boxes are quite small and there are multiple different objects to be detected.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the PCBs on a conveyor belt dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of development board PCB assemblies in various orientations and configurations on a conveyor belt</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>3 (mmp2, z3, zp3)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>1,117</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>140</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>640x640x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="826" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-zonefirst-pcbas.png" width="1688" /><figcaption><span style="white-space: pre-wrap;">Image examples from the PCBs on a conveyor belt dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/pcbs_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the PCBs on a conveyor belt dataset</span></figcaption></figure><p><strong>float32:</strong> Performance leadership split by size: YOLO&#x2011;Pro topped the small and large models, while YOLOv11 performed best at nano and medium. YOLOv5 again occupied third place throughout.</p><p><strong>int8:</strong> Quantized results favoured YOLO&#x2011;Pro at every size. YOLOv11 showed catastrophic degradation for this dataset across the entire range of model sizes.</p><hr /><h3 id="trail-camera-dataset">Trail camera dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro performs well with datasets that include varying light conditions and a mixture of color and graycale images.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the trail camera dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of animals from an outdoor trail camera at different times of the day and multiple locations</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>2 (deer, hog)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>1,180</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>131</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3 (nano, small); 640x640x3 (medium, large)</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1010" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-trail-camera.png" width="1686" /><figcaption><span style="white-space: pre-wrap;">Image examples from the trail camera dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/trail_camera_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the trail camera dataset</span></figcaption></figure><p><strong>float32:</strong> YOLO&#x2011;Pro performed best at the medium and large sizes, while YOLOv11 led at nano and small. YOLOv5 was more competitive at larger sizes but lagged at the smaller ones.</p><p><strong>int8:</strong> YOLO&#x2011;Pro maintained clear leadership after quantization. YOLOv11 failed at the medium and large sizes, and YOLOv5 and YOLOv11 swapped relative positions between nano and small.</p><hr /><h3 id="trash-bin-in-urban-settings-dataset">Trash bin in urban settings dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro can be used where objects are partially obscured, where the viewing angle of the object changes, and where the background is varied and diverse.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the trash bin in urban settings dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of trash bins, viewed from different angles, in various outdoor settings</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>1 (container)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>2,126</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>1,164</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1282" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-trash-bin-in-urban-settings.png" width="1682" /><figcaption><span style="white-space: pre-wrap;">Image examples from the trash bin in urban settings dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/trash_bin_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the trash bin in urban settings dataset</span></figcaption></figure><p><strong>float32:</strong> YOLOv11 consistently achieved the highest mAP across all sizes, with YOLOv5 following closely and YOLO&#x2011;Pro remaining competitive, particularly at the smaller scales.</p><p><strong>int8:</strong> Quantization exposed a sharp inflection: YOLOv11 remained strong at nano and small but broke down at medium and large. YOLO&#x2011;Pro surpassed YOLOv5 at all model sizes after quantization and led at the larger sizes.</p><hr /><h3 id="washers-and-bolts-dataset">Washers and bolts dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro is robust to varying backgrounds and can also generalize to different perspectives and sizes of the same object.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the washers and bolts dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images of bolts, nuts, and washers on various worktops in different orientations and lighting conditions</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>3 (bolt, nut, washer)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>284</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>72</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>320x320x3 (nano, small); 640x640x3 (medium, large)</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1236" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-washer-and-bolts.png" width="1678" /><figcaption><span style="white-space: pre-wrap;">Image examples from the washers and bolts dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/washers_and_bolts_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the washers and bolts dataset</span></figcaption></figure><p><strong>float32:</strong> YOLOv11 led at the nano and small sizes and remained competitive elsewhere, while YOLO-Pro led at the medium and large model sizes. YOLOv5 showed stronger results at medium and large than at the smaller sizes.</p><p><strong>int8:</strong> YOLO&#x2011;Pro was the most reliable option overall, leading at nano, medium, and large model sizes. Only the small YOLOv11 model retained strong performance after quantization, while the others failed. </p><hr /><h3 id="worker-assistant-dataset">Worker assistant dataset</h3><p><strong>Takeaway: </strong>YOLO-Pro is suitable for using for augmented reality applications in industrial use cases.</p>
<!--kg-card-begin: html-->
<table class="custom-table ct-dataset">
  <caption>Overview of the worker assistant dataset</caption>
  <tbody>
    <tr>
      <td>Description</td>
      <td>Images taken from first person perspective of various industrial parts being held in the user&apos;s hand</td>
    </tr>
    <tr>
      <td>Objects</td>
      <td>4 (controller, gauge, solenoid, valve)</td>
    </tr>
    <tr>
      <td>Training samples</td>
      <td>794</td>
    </tr>
    <tr>
      <td>Testing samples</td>
      <td>228</td>
    </tr>
    <tr>
      <td>Image resolution</td>
      <td>640x640x3</td>
    </tr>
  </tbody>
</table>
<!--kg-card-end: html-->
<figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="1560" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/dataset-images-worker-assistant.png" width="1690" /><figcaption><span style="white-space: pre-wrap;">Image examples from the worker assistant dataset</span></figcaption></figure><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/worker_assistant_model_comparison_test.png" width="1311" /><figcaption><span style="white-space: pre-wrap;">Model comparison results for the worker assistant dataset</span></figcaption></figure><p><strong>float32:</strong> While YOLOv11 led at the smaller scales, this dataset showed uncommon behavior: YOLOv5 topped the medium model. YOLO&#x2011;Pro led at the large size.</p><p><strong>int8:</strong> After quantization, YOLO&#x2011;Pro commanded the top spot at nano, medium, and large model sizes, while YOLOv5 unexpectedly took the lead at small. YOLOv11 failed catastrophically across all sizes.</p><h2 id="benchmark-two-real-world-projects">Benchmark two: real-world projects</h2><p>To test the performance of YOLO-Pro on a wider range of data that provides real-world relevance, we selected a sampling of ninety-five projects using the older YOLOv5 architecture from the Edge Impulse community with the curiosity of seeing if YOLO-Pro could improve the performance.</p><h3 id="methodology-1">Methodology</h3><p>We generated a list of all existing Edge Impulse projects using YOLOv5. Poorly performing projects &#x2014; those containing less than fifty training samples and those that had mAP scores lower than 0.1 &#x2014; were filtered out. From the projects remaining, ninety-five projects were randomly sampled. YOLO-Pro, YOLOv11, and YOLOv5 models were then trained on the project dataset using the same image input resolution and model size used in the original project, and performance compared across the model families. </p><p>As with the previous benchmark, the training run for each model consisted of 100 epochs, using YOLO-Pro&#x2019;s <code>No Attention with SiLU</code> architecture to match the SiLU activation function in YOLOv11 and YOLOv5. The performance of each model, assessed by mAP@[.5:.95] scores on the test samples, was calculated using float32 and int8 precision TensorFlow Lite models within Edge Impulse. The mean of the resulting mAP scores for YOLOv11 and YOLOv5 was then calculated and made relative to the YOLO-Pro results.</p><p>Further, since the parameter count for each model at a specific size is not the same across the model families, the mAP scores were normalized based on model parameter count for these project model comparisons to see the impact that the number of model parameters has on performance. For each result, the mAP result was divided by the parameter count of the model to give a mAP per parameter score, the mean mAP per parameter scores were calculated for each model family, and then the YOLOv11 and YOLOv5 results were again made relative to those from YOLO-Pro.</p><h3 id="analysis-1">Analysis</h3><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/image-1.png" width="889" /><figcaption><span style="white-space: pre-wrap;">YOLO-Pro, YOLOv11, and YOLOv5 mAP distributions across 95 projects</span></figcaption></figure><p>The figure above shows the mAP score distributions for each model family across the ninety-five sampled projects. The distributions are somewhat similar asides from the exception of the large bar on the left hand side of the YOLOv11 int8 distribution due to the quantization errors for this model architecture that have already been shown to occur. From this data, the mean mAP scores across all projects for each model family were computed and compared relative to the YOLO-Pro results. </p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="593" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/relative-map.png" width="495" /><figcaption><span style="white-space: pre-wrap;">Relative YOLOv11 and YOLOv5 mAP compared to YOLO-Pro</span></figcaption></figure><p>As can be seen in the figure above, YOLO-Pro has similar performance to YOLOv11 at float32 precision. At int8 precision, however, YOLO-Pro significantly outperforms both YOLOv11, due to the catastrophic quantization error that was demonstrated in the previous eight dataset results, and YOLOv5. </p><p>After taking into consideration model parameter count, the picture changes. YOLOv5 now has the highest mAP at float32 precisions and only slightly beats out YOLO-Pro at int8 precision. One thing to note, is that eighty out of the ninety-five sampled projects used the nano model size (a trend we have seen before; we are deploying to the edge after all), where YOLOv5 has the lowest number of parameters. If there had been more small, medium, and large models used within the sampled projects, the results may not have been skewed as much in favor of YOLOv5.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="YOLO-Pro Now Generally Available: Confidence Through Benchmarking" class="kg-image" height="592" src="https://storage.ghost.io/c/6e/0e/6e0e26fe-725b-4e15-9ec4-d601546ae1a3/content/images/2026/04/image-2.png" width="495" /><figcaption><span style="white-space: pre-wrap;">Relative YOLOv11 and YOLOv5 mAP, adjusted for parameter count, compared to YOLO-Pro</span></figcaption></figure><p>Understanding what makes YOLOv5 more efficient per parameter at the nano model size, and performing further analysis at the larger model sizes, will be an interesting future research direction. </p><h2 id="general-availability-what-changes-now">General availability: what changes now</h2><p>In practical terms, not much! That&#x2019;s intentional. </p><p>YOLO&#x2011;Pro has already been available within the Edge Impulse platform and used across a wide range of projects during its developer preview phase. With general availability, the underlying models, tooling, and workflows remain the same.</p><p>What does change is primarily about confidence and commitment:</p><ul><li>YOLO&#x2011;Pro is now a trusted object detection architecture</li><li>The developer preview label has been removed</li><li>We stand behind its stability, performance, and forward compatibility</li></ul><p>Just as importantly, general availability doesn&#x2019;t mean &#x201c;done.&#x201d; We&#x2019;ll continue to evolve YOLO&#x2011;Pro based on concrete usage, new datasets, and feedback from developers deploying at the edge. If you&#x2019;re already using YOLO&#x2011;Pro, you don&#x2019;t need to change anything. And if you&#x2019;re new to it, now is a great time to start exploring the model family.</p><h2 id="what%E2%80%99s-next-for-yolo-pro">What&#x2019;s next for YOLO-Pro</h2><p>As mentioned earlier in this post, having baseline benchmark results is not the end for YOLO-Pro development. From here, the team has future plans to create model variants optimized for specific industrial object detection tasks, add support for additional model heads such as  segmentation and pose detection, and more. Along the way, we will also be listening to, and incorporating, feedback from the community. Please do share your use cases, experiences, and suggestions on where YOLO-Pro can be improved; we&#x2019;d love to hear how you&#x2019;re using the model. </p>

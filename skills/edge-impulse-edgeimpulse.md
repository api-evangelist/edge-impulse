---
name: Edgeimpulse
description: Use when building, training, and deploying machine learning models to edge devices. Reach for this skill when working with sensor data (audio, motion, images), creating impulses (ML pipelines), managing datasets, training neural networks, or deploying models to microcontrollers, Linux devices, or browsers.
metadata:
    mintlify-proj: edgeimpulse
    version: "1.0"
---

# Edge Impulse Skill

## Product summary

Edge Impulse is a web-based platform for building, training, and deploying machine learning models to edge devices. It combines a visual Studio interface with APIs, CLIs, and SDKs to enable end-to-end ML workflows without requiring deep coding expertise. Key components: **Projects** (isolated workspaces for specific tasks), **Impulses** (ML pipelines combining input blocks, processing blocks, and learning blocks), **Datasets** (training/testing data), and **Deployments** (compiled models for edge devices). Access the platform at https://studio.edgeimpulse.com. Primary documentation: https://docs.edgeimpulse.com.

## When to use

Reach for this skill when:
- **Building ML models** for edge devices (MCUs, Linux boards, browsers)
- **Working with sensor data**: audio classification, motion/vibration analysis, image recognition, object detection
- **Managing datasets**: uploading, labeling, splitting, exploring data quality
- **Training neural networks**: configuring processing blocks (DSP), learning blocks, hyperparameters
- **Deploying models**: exporting to C++, Arduino, WebAssembly, Docker, or pre-built firmware
- **Automating workflows**: using APIs/CLI for CI/CD, batch operations, or programmatic project management
- **Optimizing for hardware**: selecting quantization, compiler options, checking RAM/flash/latency estimates

## Quick reference

### Core workflow
1. **Create project** → **Collect/import data** → **Label data** → **Design impulse** → **Train model** → **Test model** → **Deploy**

### Key file paths and commands

| Task | Command/Path |
|------|--------------|
| CLI installation | `npm install -g edge-impulse-cli` |
| Connect device | `edge-impulse-daemon` |
| Upload data | `edge-impulse-uploader` |
| Forward sensor data | `edge-impulse-data-forwarder` |
| Run model on device | `edge-impulse-run-impulse` |
| API endpoint | `https://studio.edgeimpulse.com/v1` |
| API key location | Project Dashboard → API Keys (starts with `ei_`) |
| Deployment export | Studio → Deployment → Search deployment options |

### Impulse building blocks

| Block Type | Purpose | Examples |
|-----------|---------|----------|
| **Input block** | Defines data type and window parameters | Time series (audio/motion), Images |
| **Processing block** | Feature extraction (DSP) | MFCC, MFE, Spectrogram, Image, Raw data |
| **Learning block** | Neural network training | Classification, Regression, Anomaly detection, Object detection, Transfer learning |

### Deployment options

| Option | Use case | Output |
|--------|----------|--------|
| C++ Library | Custom embedded apps | Source code, portable |
| Arduino Library | Arduino boards | .ino sketches, ready-to-flash |
| Pre-built firmware | Supported dev boards | Binary, flash directly |
| Linux .eim binary | Linux devices (RPi, Jetson) | Single executable |
| WebAssembly | Browser/Node.js | JavaScript package |
| Docker container | Containerized inference | HTTP server in container |

### Authentication

| Method | Usage |
|--------|-------|
| API Key | CLI, SDKs, REST calls (header: `x-api-key: ei_...`) |
| JWT token | Web sessions, programmatic access |
| Username/password | Web login, API fallback |

## Decision guidance

### When to use X vs Y

| Decision | Use X when | Use Y when |
|----------|-----------|-----------|
| **Quantized (int8) vs Float32** | Target device has tight RAM/flash constraints; acceptable accuracy loss | Maximum accuracy needed; device has ample resources |
| **EON Compiler vs TFLite** | Minimizing RAM/flash is critical; model is supported | Standard deployment; broader compatibility needed |
| **Transfer learning vs training from scratch** | Limited training data (<1000 samples); similar domain exists | Large dataset available; unique domain requires custom features |
| **Processing block: MFCC vs MFE** | Speech/keyword spotting (MFCC captures phonetic features) | General audio classification (MFE simpler, faster) |
| **Object detection: FOMO vs YOLO** | Localization only (centroids); extreme resource constraints | Precise bounding boxes; more compute available |
| **Data split: 80/20 vs 70/30** | Smaller dataset; need more training data | Larger dataset; want robust validation |
| **Deployment: Library vs Pre-built firmware** | Custom application logic needed; unsupported board | Quick testing; fully supported board available |

## Workflow

### Typical project workflow

1. **Create project** in Studio (specify data type: time series or image)
2. **Collect data** using:
   - Connected device (CLI: `edge-impulse-daemon`)
   - Data forwarder (CLI: `edge-impulse-data-forwarder`)
   - Mobile phone (browser-based)
   - Upload existing files (Dataset tab)
3. **Label data** in Data acquisition → Dataset or Labeling queue
4. **Explore data** using Data explorer to identify clusters, outliers, mislabeled samples
5. **Create impulse** (Impulse design tab):
   - Set input block (window size, frequency, axes)
   - Add processing block (DSP feature extraction)
   - Add learning block (neural network)
6. **Configure processing block** (e.g., MFCC parameters, image resize mode)
7. **Train model** (Learning blocks tab):
   - Set epochs, learning rate, validation split
   - Enable data augmentation if needed
   - Monitor training/validation accuracy
8. **Test model** (Model testing tab):
   - Run classification on test set
   - Review confusion matrix, precision, recall, F1 score
   - Check on-device performance (RAM, flash, latency)
9. **Deploy** (Deployment tab):
   - Select target (C++, Arduino, Linux, browser, Docker)
   - Choose optimization (quantization, compiler)
   - Download or build binary
10. **Verify on device** using CLI or browser

### API workflow example

```bash
# Get JWT token
curl -X POST https://studio.edgeimpulse.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"user@example.com","password":"pass"}'

# List projects
curl https://studio.edgeimpulse.com/v1/projects \
  -H "x-api-key: ei_YOUR_API_KEY"

# Classify a sample
curl -X POST https://studio.edgeimpulse.com/v1/projects/PROJECT_ID/classify \
  -H "x-api-key: ei_YOUR_API_KEY" \
  -d @sample.json
```

## Common gotchas

- **Window size mismatch**: Ensure window size in impulse matches your sensor sampling rate and use case. Too small = insufficient features; too large = high latency.
- **Data leakage**: Don't use the same data for training and testing. Use Dataset splits to enforce 80/20 or 70/30 automatically.
- **Overfitting**: If training accuracy >> validation accuracy, reduce epochs, increase learning rate, or enable data augmentation.
- **Quantization accuracy loss**: int8 quantized models may lose 2-5% accuracy. Always test quantized models in Model testing before deployment.
- **Unsupported sensor types**: Some processing blocks only work with specific data types (e.g., MFCC for audio only). Check block compatibility.
- **API key exposure**: Never commit API keys to version control. Use environment variables or secrets management.
- **Deployment size**: C++ libraries can be large (1-10 MB). Use EON Compiler to reduce size for constrained devices.
- **Missing labels**: Unlabeled data won't be used for training. Use Data explorer to find and label gray (unlabeled) dots.
- **Frequency mismatch**: If your device samples at 16 kHz but impulse expects 8 kHz, data will be resampled, affecting model accuracy.
- **Device not connecting**: Ensure device firmware is Edge Impulse-compatible. Use `edge-impulse-daemon` to verify serial connection and proxy data.

## Verification checklist

Before submitting work or deploying a model:

- [ ] Dataset has at least 50-100 samples per class (more is better)
- [ ] Training/testing split is enforced (no data leakage)
- [ ] Model testing accuracy is acceptable (>80% for most tasks)
- [ ] Confusion matrix shows no systematic misclassifications
- [ ] On-device performance (RAM, flash, latency) fits target hardware
- [ ] Quantized model tested and accuracy acceptable (if using int8)
- [ ] Deployment option selected matches target device
- [ ] API key is not exposed in code or logs
- [ ] Impulse versioned (use Versioning tab to track changes)
- [ ] Model tested on actual device (not just in browser)

## Resources

**Comprehensive navigation**: https://docs.edgeimpulse.com/llms.txt

**Critical documentation pages**:
1. [Studio overview](https://docs.edgeimpulse.com/studio) — Core platform features and project structure
2. [Impulse design](https://docs.edgeimpulse.com/studio/projects/impulse-design) — Building ML pipelines with input, processing, and learning blocks
3. [Deployment guide](https://docs.edgeimpulse.com/studio/projects/deployment) — Exporting models to edge devices with optimization options
4. [Getting started for beginners](https://docs.edgeimpulse.com/knowledge/guides/getting-started-for-beginners) — Step-by-step workflow walkthrough
5. [Edge Impulse CLI](https://docs.edgeimpulse.com/tools/clis/edge-impulse-cli) — Command-line tools for data collection and device management
6. [Studio API](https://docs.edgeimpulse.com/apis/studio) — Programmatic access to projects, training, and deployment
7. [Hardware boards](https://docs.edgeimpulse.com/hardware/boards) — Supported development boards and deployment targets

---

> For additional documentation and navigation, see: https://docs.edgeimpulse.com/llms.txt
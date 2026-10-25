<img data-src="img/lmb_logo.webp" class="title-page-logo">


# Self-supervised spatial features for protein detection<br> in crowded cryo-ET tomograms

**Bogdan Toader**\
MRC Laboratory of Molecular Biology

<br>

CCP-EM Spring Symposium\
24 April 2026, Nottingham

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<img data-src="img/nmar_andriko.jpg" alt="N. Maritimus image from von Kugelgen paper" height="600">

Ribosomes and S-layer proteins are abundant and high-contrast,\
but what about the **smaller, rarer proteins in the crowded cellular environment**?
<!-- .element: class="fragment" -->

<a href="https://www.nature.com/articles/s41586-024-07462-5" class="citation">von Kügelgen *et al.* Nature 2024</a>

Note: some notes

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<!-- .slide: class="left" -->

Current approaches:

* Manual annotation: slow for many tomograms  <!-- .element: class="fragment" -->

* Template matching: slow (N passes for N types) + requires templates  <!-- .element: class="fragment" -->

* Deep learning models:  <!-- .element: class="fragment" -->

  * Require annotated data  <!-- .element: class="fragment" -->

  * General purpose segmentation models only suitable for large structures  <!-- .element: class="fragment" -->

  * Contrastive specialised models embed one particle at a time  <!-- .element: class="fragment" -->

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

Lots of available data

<img data-src="img/pdb_growth.webp" alt="PDB growth" height="500" class="fragment" data-fragment-index="1">
<img data-src="img/alphafold_db.jpg" alt="AlphaFold DB" height="500" class="fragment" data-fragment-index="2">

Why not use it? <!-- .element: class="fragment green" data-fragment-index="3"-->

<a href="https://www.rcsb.org/stats/growth/growth-em" class="citation fragment" data-fragment-index="1">https://www.rcsb.org/stats/growth/growth-em</a>
<a href="https://alphafold.com" class="citation fragment" data-fragment-index="2">https://alphafold.com</a>

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<!-- .slide: class="left" -->


Self-supervised learning on simulated cryo-ET data: **TomoFind** <!-- .element: class="tfblue" -->
* Requires annotated data <!-- .element: class="fragment fade-in-then-out"-->
  * “Infinite” amount of simulated data <!-- .element: class="fragment green"-->

* General purpose segmentation models only suitable for large structures<!-- .element: class="fragment fade-in-then-out"-->
  * Train with noisy, corrupted data <!-- .element: class="fragment green"-->

* Contrastive specialised models embed one particle at a time<!-- .element: class="fragment fade-in-then-out"-->
  * Simulate crowding in cellular environment<!-- .element: class="fragment green"-->

<br><br>

**Long-term goal:** template (and hassle)-free identification of structures in tomograms
<!-- .element: class="fragment"-->

---

<!-- .slide: style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data
2. Crowding robustness
3. Template-free detection + classification example

<!-- .element: style="line-height: 2.0" -->

---

<!-- .slide: style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="listcurrent" -->
2. Crowding robustness <!-- .element: class="gray" -->
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---
### SSL on simulated cryo-ET data: data simulation pipeline

<div class="grid cols-4" style="font-size: 0.9em; row-gap: 0">
<p>Place particles in volume<br>at random positions</p>
<p>Generate crowded environment</p>
<p>Project + add noise<br>to tilt images</p>
<p>Reconstruct<br>tomogram</p>
<video data-src="img/data_simulation/big_only_zslices.mp4" data-autoplay loop muted></video>
<video data-src="img/data_simulation/clean_zslices.mp4" data-autoplay loop muted></video>
<video data-src="img/data_simulation/tilts_noisy_dose_weighted.mp4" data-autoplay loop muted></video>
<video data-src="img/data_simulation/simulated_zslices.mp4" data-autoplay loop muted></video>
</div>

<a href="https://www.nature.com/articles/s41592-023-01878-z" class="citation">~130 PDB_IDs from Rice, G., Wagner *et al.* Nat Methods 2023</a>

---
### SSL on simulated cryo-ET data: encoder

<img data-src="img/model_diagram.webp" alt="Model diagram" height="750">

---
### SSL on simulated cryo-ET data: multi-view training

<img data-src="img/training_diagram.webp" alt="Model diagram" height="550">

<div class="turqoise-box">

Train the model:  
1. Match feature vectors of all local views to those of all global views 
2. Force features from all input blocks in the batch to separate to avoid collapse

</div>

---
### SSL on simulated cryo-ET data: resulting model

<video data-src="img/input_encoder_features.mp4" height="500" data-autoplay loop muted></video>

**Particle embedding:**  feature vector at particle position
<!-- .element: class="fragment" -->

<br>

<div class="cool-box fragment">

The feature volume encodes spatial position AND protein identity.

</div>

---

<!-- .slide: style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="gray" -->
2. Crowding robustness <!-- .element: class="listcurrent" -->
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---

<!-- .slide: style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="gray" -->
2. Crowding robustness <!-- .element: class="listcurrent" --><br>
  Q1: Are particle embeddings robust to crowding?<br>
  Q2: Do embeddings distinguish different particle types in real data?<br>
  Q3: Can we detect particles in tomograms?<br>
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

<!-- PDF p. 14 -->

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

1. **Simulate** volumes at different density levels (1-17 particles/volume)

   <img data-src="img/crowding/crowding_inputs.webp" alt="Increasingly crowded inputs" height="270">

2. **Compute particle embeddings** using:
* **TomoFind**<!-- .element: class="tfblue" -->: trained with multiple particles/block
* **Baseline**<!-- .element: class="orange" -->: trained with one particle/block

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

<div class="grid" style="grid-template-columns: auto 1fr">
<span>Particles/block</span>
<div class="grid cols-4" style="gap: 0; position: relative; left: 0.8%"><span>1-3</span><span>4-6</span><span>7-9</span><span>10-17</span></div>
<strong class="tfblue">TomoFind</strong>
<img data-src="img/crowding/crowding_umap_tomofind.webp" alt="UMAP of TomoFind embeddings in increasingly crowded inputs">
<strong class="orange">Baseline</strong>
<img data-src="img/crowding/crowding_umap_baseline.webp" alt="UMAP of baseline embeddings in increasingly crowded inputs">
</div>

* Axes: UMAP 2 dimensions
* Colours: ground truth labels of 10 particle types

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

Classify each particle using  `KNeighborsClassifier`\
Particle type = type of majority of k-closest neighbours

<img data-src="img/crowding/accuracy_vs_crowding.webp" alt="Accuracy vs croding graphs showing TomoFind stays roughly flat while Baseline decreases" height="600">

<br>

* **TomoFind**<!-- .element: class="tfblue" -->: trained with multiple particles/block
* **Baseline**<!-- .element: class="orange" -->: trained with one particle/block

---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?

<!-- PDF p. 18 -->

---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?

**Dataset:** CZII Object Identification Challenge public test dataset DS-10445

* 32,000 particles, 121 tomograms at 10Å/voxel 

* 6 particle types: 

<br>

<img data-src="img/particle_types.webp" alt="Particle types" height="200">

Compare embeddings of **TomoFind**<!-- .element: class="tfblue" --> & **TomoTwin**<!-- .element: class="orange" --> **at ground-truth coordinates**

<a href="https://www.nature.com/articles/s41592-023-01878-z" class="citation">~130 PDB_IDs from Rice, G., Wagner *et al.* Nat Methods 2023</a>

---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?

<br>

<div class="grid" style="grid-template-columns: 0.6fr 0.6fr 0.4fr; row-gap:0">
<strong class="tfblue" style="position: relative; left:5%">TomoFind</strong>
<strong class="orange" style="position: relative; left:4%">TomoTwin</strong>
<p></p>
<img data-src="img/classification/classification_filtered_tomofind.webp" alt="Classification using TomoFind on filtered particles" style="scale:100%">
<img data-src="img/classification/classification_filtered_tomotwin.webp" alt="Classification using TomoTwin on filtered particles" style="scale:100%">
<p style="font-size: 0.8em" >Coloured by ground-truth particle types:
<img data-src="img/particle_types.webp" alt="Particle types" style="scale:70%">
</p>
</div>

<br>


---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?

<br>

<div class="grid" style="grid-template-columns: 0.6fr 0.6fr 0.4fr; row-gap:0">
<strong class="tfblue" style="position: relative; left:4%">TomoFind</strong>
<strong class="orange" style="position: relative; left:3%">TomoTwin</strong>
<p></p>
<img data-src="img/classification/classification_denoised_tomofind.webp" alt="Classification using TomoFind on denoised particles" style="scale:100%">
<img data-src="img/classification/classification_denoised_tomotwin.webp" alt="Classification using TomoTwin on denoised particles" style="scale:100%">
<p style="font-size: 0.8em" >Coloured by ground-truth particle types:
<img data-src="img/particle_types.webp" alt="Particle types" style="scale:70%">
</p>
</div>

<p class="magenta" style="font-size:1.2em">
Bonus: <strong class="tfblue">TomoFind</strong> also works on denoised tomograms
</p>

<br>

---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?


<p style="font-size:1.2em">Classification accuracy</p>

<br>


| Processing type | <strong class="tfblue">TomoFind</strong> | <strong class="orange">TomoTwin</strong> |
|---|---|---|
| Filtered | 0.90 | 0.88 |
| Denoised | 0.89 | 0.82 | 


<br><br>

<p class="magenta" style="font-size:1.2em">
<strong class="tfblue">TomoFind</strong> computes consistent embeddings regardless of preprocessing choice
</p>


<!-- TODO: center align of text in table -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<!-- PDF p. 23 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<div class="grid cols-2">
<span>Tomogram</span>
<strong class="tfblue">TomoFind features</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 24 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<div class="grid cols-3">
<span>Tomogram</span>
<strong class="tfblue">TomoFind features</strong>
<strong class="orange">TomoTwin features</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 25 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<!-- .slide: class="left" -->

TomoTwin-like **reference based** workflow:

1. Compute tomogram feature volume

2. Pick N=5 random references per type & compute embeddings

3. At every voxel in the feature volume, compute minimum distance to any reference\
   &emsp;=> distance volume

4. Find local minima in the distance volume, grouped by nearest particle type\
   &emsp;=> candidate detections / type

<!-- PDF p. 26 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<div class="grid cols-2">
<strong class="tfblue">TomoFind detections</strong>
<strong class="orange">TomoTwin detections</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 27 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<!-- .slide: class="left" -->

Precision-recall curves for **TomoFind**<!-- .element: class="tfblue" --> and **TomoTwin**<!-- .element: class="orange" -->
* Sweep distance threshold: keep peaks below threshold, match to ground-truth coordinates
* At each threshold, count true positives (TP) vs ground-truth detections (GT)
* **Recall** = fraction of real particles found (TP / all GT)
* **Precision** = fraction of detections that are real (TP / all detected)

<div class="grid cols-4" style="text-align: center">
<span>Ribosome (84 particles)</span>
<span>VLP (39 particles)</span>
<span>Ferritin (86 particles)</span>
<span>Thyroglobulin (102 particles)</span>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 28 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<div class="grid cols-3">
<span>Tomogram</span>
<strong class="tfblue">TomoFind features</strong>
<strong class="orange">TomoTwin features</strong>
</div>

<div class="cool-box">

Idea: reference-free particle detection

</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 29 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

**Reference-free particle detection using feature magnitude**<!-- .element: class="magenta" -->

<div class="grid cols-3">
<span>Tomogram</span>
<strong class="tfblue">TomoFind feature norm</strong>
<strong class="orange">TomoTwin feature norm</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 30 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms?

<!-- .slide: class="left" -->

<p class="magenta" style="text-align: center"><strong>Reference-free particle detection</strong></p>

**Norm-based**<!-- .element: class="magenta" --> workflow:

1. Compute tomogram feature volume

2. Compute norm of feature vector at every voxel => detection volume

3. Find local maxima in the detection volume\
   &emsp;=> **reference-free candidate detections**<!-- .element: class="magenta" -->

Validate by computing distance from detections to references and sweeping\
distance threshold as before to compute per-type precision-recall curves.

<!-- PDF p. 31 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms <span class="magenta">reference-free</span>?

**<span class="tfblue">TomoFind’s</span> spatial features detect particles using feature magnitude**

PR curves: norm-located peaks + dist to reference sweep

<div class="grid cols-4">
<span>Ribosome (84 particles)</span>
<span>VLP (39 particles)</span>
<span>Ferritin (86 particles)</span>
<span>Thyroglobulin (102 particles)</span>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19159</a>

<!-- PDF p. 32 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms <span class="magenta">reference-free</span>?

<div class="grid cols-3">
<span>Tomogram</span>
<strong class="tfblue">TomoFind features</strong>
<strong class="orange">TomoTwin features</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19099</a>

<!-- PDF p. 33 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms <span class="magenta">reference-free</span>?

<div class="grid cols-3">
<span>Tomogram</span>
<strong class="tfblue">TomoFind feature norm</strong>
<strong class="orange">TomoTwin feature norm</strong>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19099</a>

<!-- PDF p. 34 -->

---
### Crowding robustness Q3: Can we detect particles in tomograms <span class="magenta">reference-free</span>?

Ferritin (771 particles)

<div class="grid cols-2">
<span>Minimum distance detection</span>
<span>Feature norm detection</span>
</div>

<a href="https://cryoetdataportal.czscience.com/datasets/10445" class="citation">CZII Cryo-ET Portal, dataset 10445, tomogram 19099</a>

<!-- PDF p. 35 -->

---

<!-- .slide: style="font-size: 1.3em" -->

1. SSL on simulated cryo-ET data  <!-- .element: class="gray" -->
2. Crowding robustness <!-- .element: class="gray" -->
3. Template-free detection + classification example <!-- .element: class="listcurrent" -->

<!-- .element: style="line-height: 2.0" -->

---
### Template-free detection + classification

* Tomogram of a *Nitrosopumilus maritimus* cell (von Kügelgen *et al.* Nature 2024)
* Features: ribosomes, S-layer proteins and other things (fiducials, carbon grid)

<a href="https://www.nature.com/articles/s41586-024-07462-5" class="citation">von Kügelgen *et al.* Nature 2024</a>

<!-- PDF p. 37 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span>1. Apply TomoFind encoder to tomogram</span><span>=> feature volume</span>
<span>2. Compute feature norm at feature voxels</span><span>=> detection volume</span>
<span>3. Find peaks, extract embeddings and cluster</span><span>=> particle candidates</span>
</div>

<!-- PDF p. 38 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<strong>1. Apply TomoFind encoder to tomogram</strong><strong>=> feature volume</strong>
<span class="gray">2. Compute feature norm at feature voxels</span><span class="gray">=> detection volume</span>
<span class="gray">3. Find peaks, extract embeddings and cluster</span><span class="gray">=> particle candidates</span>
</div>

<!-- PDF p. 39 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<strong>1. Apply TomoFind encoder to tomogram</strong><strong>=> feature volume</strong>
<span class="gray">2. Compute feature norm at feature voxels</span><span class="gray">=> detection volume</span>
<span class="gray">3. Find peaks, extract embeddings and cluster</span><span class="gray">=> particle candidates</span>
</div>

<!-- PDF p. 40 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span class="gray">1. Apply TomoFind encoder to tomogram</span><span class="gray">=> feature volume</span>
<strong>2. Compute feature norm at feature voxels</strong><strong>=> detection volume</strong>
<span class="gray">3. Find peaks, extract embeddings and cluster</span><span class="gray">=> particle candidates</span>
</div>

<!-- PDF p. 41 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span class="gray">1. Apply TomoFind encoder to tomogram</span><span class="gray">=> feature volume</span>
<strong>2. Compute feature norm at feature voxels</strong><strong>=> detection volume</strong>
<span class="gray">3. Find peaks, extract embeddings and cluster</span><span class="gray">=> particle candidates</span>
</div>

<!-- PDF p. 42 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span class="gray">1. Apply TomoFind encoder to tomogram</span><span class="gray">=> feature volume</span>
<span class="gray">2. Compute feature norm at feature voxels</span><span class="gray">=> detection volume</span>
<strong>3. Find peaks, extract embeddings and cluster</strong><strong>=> particle candidates</strong>
</div>

<!-- PDF p. 43 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span class="gray">1. Apply TomoFind encoder to tomogram</span><span class="gray">=> feature volume</span>
<span class="gray">2. Compute feature norm at feature voxels</span><span class="gray">=> detection volume</span>
<strong>3. Find peaks, extract embeddings and cluster</strong><strong>=> particle candidates</strong>
</div>

<!-- PDF p. 44 -->

---
### Template-free detection + classification

<div class="grid" style="grid-template-columns: auto auto; row-gap: 0; text-align: left">
<span class="gray">1. Apply TomoFind encoder to tomogram</span><span class="gray">=> feature volume</span>
<span class="gray">2. Compute feature norm at feature voxels</span><span class="gray">=> detection volume</span>
<strong>3. Find peaks, extract embeddings and cluster</strong><strong>=> particle candidates</strong>
</div>

<!-- PDF p. 45 -->

---
### Summary and outlook

<!-- .slide: class="left" -->

**TomoFind**<!-- .element: class="tfblue" -->

* Trained entirely on simulated cryo-ET data
* Spatial features robust to crowding, missing wedge, noise and preprocessing method
* Single forward pass for joint detection and classification of particles

<br>

<div class="grid" style="grid-template-columns: 1fr auto 1fr auto 1fr; text-align: center; font-size: 0.8em">
<p><strong>Today</strong><br>Detection and classification of known particles, trained on simulated data</p>
<span style="font-size: 2em">→</span>
<p><strong>Next</strong><br>Improved data diversity and PDB-grounded identification<br>=> cryo-ET foundation model</p>
<span style="font-size: 2em">→</span>
<p><strong>Goal</strong><br><strong><em class="orange">“One model to rule them all”</em></strong><br>Fast template-free discovery of all relevant structures in a single pass through the data</p>
</div>

<!-- PDF p. 46 -->

---
### Acknowledgements

<div class="grid" style="grid-template-columns: auto auto; column-gap: 3em; text-align: left; font-size: 0.8em">
<div>
<p><strong>Scheres &amp; Bharat labs</strong></p>
<div class="grid cols-2" style="align-items: start">
<p>Sjors Scheres</p>
<p>Tanmay Bharat</p>
<p>Yeongjin Baek<br>Chloe Fisher<br>Melissa Huang<br>Kiarash Jamali<br>Vinish Kishore<br>Simon Knoblauch<br>Sofia Lövestam<br>Jennifer Macdonald<br>Kai Yi<br>Dongqi Zhang</p>
<p>Jan Böhning<br>Ido Caspy<br>Camila Clemente<br>Julia Gordeeva<br>Rebecca Hankins<br>Buse Isbilir<br>Kenny Jungfer<br>Eloise Mawdesley<br>Hannah Ochner<br>Olivia Smith<br>James Stacey<br>Abul Tarafder<br>Andriko von Kügelgen<br>Zhexin (Eric) Wang</p>
</div>
</div>
<div>
<p><strong>Scientific Computing @ MRC LMB</strong></p>
<p><strong>Isambard-AI National AI Research Resource (AIRR)</strong></p>
</div>
</div>

<!-- PDF p. 47 -->

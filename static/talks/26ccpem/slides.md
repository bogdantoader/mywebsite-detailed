<img src="img/lmb_logo.png" class="title-page-logo">


# Self-supervised spatial features for protein detection<br> in crowded cryo-ET tomograms

**Bogdan Toader**\
MRC Laboratory of Molecular Biology

<br>

CCP-EM Spring Symposium\
24 April 2026, Nottingham

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<br>

<img src="img/nmar_andriko.jpg" alt="N. Maritimus image from von Kugelgen paper" class="r-stretch">

Ribosomes and S-layer proteins are abundant and high-contrast,\
but what about the **smaller, rarer proteins in the crowded cellular environment**?
<!-- .element: class="fragment" -->

<br>

<a href="https://www.nature.com/articles/s41586-024-07462-5" class="citation">von Kügelgen *et al.* Nature 2024</a>


Note: some notes

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<!-- .slide: class="left" -->

<br><br>

Current approaches:

* Manual annotation: slow for many tomograms  <!-- .element: class="fragment" -->

* Template matching: slow (N passes for N types) + requires templates  <!-- .element: class="fragment" -->

* Deep learning models:  <!-- .element: class="fragment" -->

  * Require annotated data  <!-- .element: class="fragment" -->

  * General purpose segmentation models only suitable for large structures  <!-- .element: class="fragment" -->

  * Contrastive specialised models embed one particle at a time  <!-- .element: class="fragment" -->

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<!-- .slide: class="middle" -->

Lots of available data

![PDB growth](img/pdb_growth.png) <!-- .element: height="500" class="fragment" data-fragment-index="1"-->
![AlphaFold DB](img/alphafold_db.jpg) <!-- .element: height="500" class="fragment" data-fragment-index="2"-->

Why not use it? <!-- .element: class="fragment green" data-fragment-index="3"-->

<a href="https://www.rcsb.org/stats/growth/growth-em" class="citation fragment" data-fragment-index="1">https://www.rcsb.org/stats/growth/growth-em</a>
<a href="https://alphafold.com" class="citation fragment" data-fragment-index="2">https://alphafold.com</a>

---
### Challenge: comprehensive identification of structures in cryo-ET tomograms

<!-- .slide: class="left" -->

<br>

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

<!-- .slide: class="center" style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data
2. Crowding robustness
3. Template-free detection + classification example

<!-- .element: style="line-height: 2.0" -->

---

<!-- .slide: class="center" style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="listcurrent" -->
2. Crowding robustness <!-- .element: class="gray" -->
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---
### SSL on simulated cryo-ET data: data simulation pipeline

<!-- .slide: class="middle" -->

<div class="grid cols-4" style="font-size: 0.9em; row-gap: 0">
<p>Place particles in volume<br>at random positions</p>
<p>Generate crowded environment</p>
<p>Project + add noise<br>to tilt images</p>
<p>Reconstruct<br>tomogram</p>
<video src="img/data_simulation/big_only_zslices.mp4" data-autoplay loop muted></video>
<video src="img/data_simulation/clean_zslices.mp4" data-autoplay loop muted></video>
<video src="img/data_simulation/tilts_noisy_dose_weighted.mp4" data-autoplay loop muted></video>
<video src="img/data_simulation/simulated_zslices.mp4" data-autoplay loop muted></video>
</div>


<a href="https://www.nature.com/articles/s41592-023-01878-z" class="citation">
~130 PDB_IDs from Rice, G., Wagner, et al. Nat Methods 2023
</a>


---
### SSL on simulated cryo-ET data: encoder

<!-- .slide: class="middle" -->

<img src="img/model_diagram.png" alt="Model diagram" height="750">


---
### SSL on simulated cryo-ET data: multi-view training

<!-- .slide: class="middle" -->

<img src="img/training_diagram.png" alt="Model diagram" height="550">


<div class="turqoise-box">

Train the model:  
1. Match feature vectors of all local views to those of all global views 
2. Force features from all input blocks in the batch to separate to avoid collapse

</div>

---
### SSL on simulated cryo-ET data: resulting model


<!-- .slide: class="middle" -->

<div class="r-hstack" style="gap: 40px">
<video src="img/learned_features/input_zslices_sample000_local.mp4" height="500" data-autoplay loop muted></video>
<img src="img/learned_features/features_encoder.png" alt="Model diagram" height="300">
<video src="img/learned_features/feature_pca_zslices_sample000_local.mp4" height="500" data-autoplay loop muted></video>
</div>

**Particle embedding:**  feature vector at particle position
<!-- .element: class="fragment" -->

<br>

<div class="cool-box fragment">
The feature volume encodes spatial position AND protein identity.
</div>

---

<!-- .slide: class="center" style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="gray" -->
2. Crowding robustness <!-- .element: class="listcurrent" -->
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---

<!-- .slide: class="center" style="font-size: 1.3em" -->

1. Self-supervised learning on simulated cryo-ET data  <!-- .element: class="gray" -->
2. Crowding robustness <!-- .element: class="listcurrent" --><br>
  Q1: Are particle embeddings robust to crowding?<br>
  Q2: Do embeddings distinguish different particle types in real data?<br>
  Q3: Can we detect particles in tomograms?<br>
3. Template-free detection + classification example <!-- .element: class="gray" -->

<!-- .element: style="line-height: 2.0" -->

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

<!-- .slide: class="middle" -->

1. **Simulate** volumes at different density levels (1-17 particles/volume)

   <img src="img/crowding/crowding_inputs.png" alt="Increasingly crowded inputs" height="270">

2. **Compute particle embeddings** using:
* **TomoFind**<!-- .element: class="tfblue" -->: trained with multiple particles/block
* **Baseline**<!-- .element: class="orange" -->: trained with one particle/block

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

<!-- .slide: class="middle" -->

<div class="grid" style="grid-template-columns: auto 1fr">
<span>Particles/block</span>
<div class="grid cols-4" style="gap: 0; position: relative; left: 0.8%"><span>1-3</span><span>4-6</span><span>7-9</span><span>10-17</span></div>
<strong class="tfblue">TomoFind</strong>
<img src="img/crowding/crowding_umap_tomofind.png" alt="UMAP of TomoFind embeddings in increasingly crowded inputs">
<strong class="orange">Baseline</strong>
<img src="img/crowding/crowding_umap_baseline.png" alt="UMAP of baseline embeddings in increasingly crowded inputs">
</div>

* Axes: UMAP 2 dimensions
* Colours: ground truth labels of 10 particle types

---
### Crowding robustness Q1: Are particle embeddings robust to crowding?

<!-- .slide: class="middle" -->

Classify each particle using  `KNeighborsClassifier`\
Particle type = type of majority of k-closest neighbours

<img src="img/crowding/accuracy_vs_crowding.png" alt="Accuracy vs croding graphs showing TomoFind stays roughly flat while Baseline decreases" height="600">

<br>

* **TomoFind**<!-- .element: class="tfblue" -->: trained with multiple particles/block
* **Baseline**<!-- .element: class="orange" -->: trained with one particle/block

---
### Crowding robustness Q2: Do embeddings distinguish different particle types in real data?

To be continued...

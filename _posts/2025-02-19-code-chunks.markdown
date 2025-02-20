---
layout: post
title:  "Organizing fMRI files with Python!"
date:   2025-02-19 12:00:00 -0800
categories: jekyll update
---

<body style="font-family: Optima">

A common issue in neuroimaging data anlaysis is "ballooning" of the data throughout the analysis process. For example, one must convert raw DICOM files from k-space into volumetric space, remove unstable images from the beginning of data acquisition, realign functional images with the high-resolution structural image, warp the data into a standarized sterotaxic space (e.g., MNI), and then complete spatial smoothing - and all of this only covers the pre-processing stage! If you write out files from each step of processing (which can be useful for diagnosing issues), you will likely need to shuffle things around at some point by transferring the "final" preprocessed data to a new location. The code below does just that, while preserving the original directory structure of the source directory. 
<br>
<br>
<pre><code class="language-python">import os
import shutil

def copy_vtc_files(source_folder, destination_folder):
    for root, _, files in os.walk(source_folder):
        relative_path = os.path.relpath(root, source_folder)
        dest_dir = os.path.join(destination_folder, relative_path)
    
        os.makedirs(dest_dir, exist_ok = True)
    
        for file in files:
            if file.endswith(".rtv"):
                src_file = os.path.join(root, file)
                dest_file = os.path.join(dest_dir, file)
                shutil.copy2(src_file, dest_file)
                
source = "/Volumes/G-DRIVE_Thunderbolt_3/2023_PDC3_validation/PreTx/data/PostTx/data/Imaging/"
destination = "/Users/nick/Documents/PDC3_PreprocPostTx/"
copy_vtc_files(source, destination)

print(f"Copied all .vtc files from {source} to {destination}")
</code></pre>


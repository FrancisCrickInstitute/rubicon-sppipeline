# rubicon-sppipeline
Nextflow pipeline for IMC spatial data methods.

# Inputs
- `cell_objects.csv`
    - A plaintext, delimited file containing single cell-level coordinate data for a set of images, plus their phenotypic identities.
- `metadata.csv`
    - A plaintext, delimited file containing metadata information about the images in `cell_objects.csv`. To run the pipeline this file must contain,f or each image, an image identifier ('imagename'), and the width and height in pixels for every image as columns with the header `'image_width'` and `'image_height'`.

# Workflow options
Spatial-PHLEX can be run in multiple modes:
- 'default'
    - The default pipeline. This performs density-based spatial clustering of all cell types in the objects dataframe. Subsequently, graph-based barrier scoring is executed for a defined, source (e.g. CD8 T cells), target (e.g. Epithelial cells), and barrier (e.g. Myofibroblasts) cell type.
- 'spatial_clustering'
    - Performs only spatial clustering, for all cell types identified in the cell objects dataframe.
- 'barrier_only'
    - Performs graph-based barrier scoring without performing spatial clustering.

# Example Usage

```bash
nextflow run ./main.nf \
    --metadata '/camp/project/proj-tracerx-lung/tctProjects/rubicon/tracerx/master_files/metadata/metadata.tracerx.txt'\
    --metadata_delimiter '\t'\
    --objects "/camp/project/proj-tracerx-lung/tctProjects/rubicon/tracerx/tx100/imc/outputs/cell_typing/tx100_cell_objects_tx100_publication_p1.txt" \
    --objects_delimiter '\t'\
    --PANEL 'p1' \
    --phenotyping_level 'cellType' \
    --barrier_phenotyping_level 'cellType' \
    --graph_conda_env "/camp/lab/swantonc/working/Alastair/.conda/envs/rapids-22.02" \
    --graph_type 'nearest_neighbour' \
    --md_conda 'Anaconda3' \
    --outdir '../../results' \
    --release '2022-08-30_DSL2_dev_b' \
    --spclust_conda_env "/camp/lab/swantonc/working/Alastair/.conda/envs/tf" \
    --workflow_name 'default' \
    --dev \
    -resume
```
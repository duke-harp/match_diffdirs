`match_diffdirs.sif` is a Singularity container that takes input and reference
diffusion images (or merely a list of the diffusion gradient directions and
strengths corresponding to said images).
The output is a subset of the input image volumes (or list of diffusion
directions) that best matches to the reference data with respect to the
diffusion gradients.
This is useful, for example, when two subject cohorts are acquired with
different diffusion acquisition parameters, especially extra diffusion
directions and/or extra shells.
Each volume/direction in the input dataset is chosen because its diffusion
vector is the closest (by dot product) to a diffusion vector in the reference
data set.

## Usage
  `match_diffdirs.sif [other options] IN REF OUT`

Selects volumes (or elements) from a diffusion image (or list of diffusion
directions) IN that best match the gradient directions in REF, and writes
them to OUT.

Typically, IN specifies a set of diffusion images sampled at various
gradient directions and strengths and REF defines a near subset (or a
shell) of those diffusion directions, and OUT represents the closest
match to REF's subset.  Images or values representing B0 volumes are
always included in the output.

IN, REF, and OUT each represent one or both of options of the form:

-  `--PREFIX_image   IMAGE`
-  `--PREFIX_fslgrad BVECS BVALS`

where PREFIX is 'in', 'ref', or 'out'.  Each of IN and REF must somehow
specify diffusion gradient directions.  For example if `--in_image` points
to a .nii.gz file (which does not store diffusion directions), you must
also specify the `--in_fslgrad` option with FSL-style bvecs and bvals
files.  Otherwise diffusion gradient directions can be extracted from
image files like .mif and .bxh.  If you specify an input image with
`--in_image`, then you may use `--out_image` to write the selected volumes.
Otherwise only `--out_fslgrad` is supported and only the closest matching
diffusion directions are written to the output files.

Other options:
  `--numb0 NUM`
     By default, all B0 volumes (defined as bval<50 or ||bvec||<0.1)
     are retained in the output.  If --numb0 is specified, only the
     first NUM B0 volumes are kept.

If `--overwrite` is specified, any output files that already exist will be
overwritten without warning.

If `--writebids` (and `--out_image`) is specified, a BIDS-compliant .json and
.tsv file will also be written.


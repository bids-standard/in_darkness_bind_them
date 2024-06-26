<!-- the section below is automatically generated.

If you want to modify the questions:
- please edit the files in the `src/questions` folder.
- run `faqtory build` from the root of the repository.

-->

## Can I combine BIDS and neurodata without border (NWB)?

BIDS and [NWB](https://www.nwb.org/) are compatible.

An NWB data file is an allowed format in the iEEG-BIDS data structure.
This means that one subject (AAA) with a session (BBB)
can have a BIDS folder with raw iEEG data in NWB format:

```text
/sub-AAA/ses-BBB/ieeg/sub-AAA_ses-BBB_task-rest_ieeg.nwb
```

The same subject can have another session (CCC) with raw fMRI data in BIDS:

```text
/sub-AAA/ses-CCC/func/sub-AAA_ses-CCC_task-rest_bold.nii.gz
```

## How can I cite BIDS?

See the specification website for the
[main publications](https://bids-specification.readthedocs.io/en/latest/01-introduction.html#citing-bids)
related to BIDS and its extensions.

BIDS references are centralized in a [zotero group](https://www.zotero.org/groups/5111637/bids).

## How do I convert my data to BIDS?

We strongly recommend you pick a BIDS converter to help you convert your data.

A list of converters can be found [on the BIDS website](https://bids.neuroimaging.io/benefits.html#converters)

Also look at [the list of tutorials and information about conversions](../starter_kit/src/tutorials/tutorials.md).

## How should I organize data for hyperscanning data?

Hyperscanning is simultaneous fMRI with multiple subjects (see this [paper](https://doi.org/10.1006/nimg.2002.1150)).

-   See this [issue](https://github.com/bids-standard/bids-specification/issues/402)
  in the bids specification repository for typical hyperscanning data.

See an example below with fMRI data:

```text
sub-01/
    ses-dyadic1/
        func/
            sub-01_ses-dyadic1_*
sub-02/
    ses-dyadic1/
        func/
            sub-02_ses-dyadic1_*
sub-03/
    ses-dyadic2/
        func/
            sub-03_ses-dyadic2_*
sub-04/
    ses-dyadic2/
        func/
            sub-04_ses-dyadic2_*
```

-   See this [post on neurostars](https://neurostars.org/t/bids-structure-for-longitudinal-dyadic-data/26173)
    for hyperscanning longitunal data.

See an example below with fMRI data:

```text
sub-S001/
    ses-1/
        func/
            sub-S001_ses-1_task-video_acq-dyad001_bold.nii.gz
    ses-2/
sub-S002/
    ses-1/
        func/
            sub-S002_ses-1_task-video_acq-dyad001_bold.nii.gz
    ses-2/
sub-S003/
    ses-1/
        func/
            sub-S003_ses-1_task-video_acq-dyad002_bold.nii.gz
    ses-2/
```

-   See this [thread on the bids discussion forum](https://groups.google.com/g/bids-discussion/c/v660DuzOf3w/m/q-0PLHt5BgAJ)

## How to import Excel files to TSV file?

See [our sections on TSV files](../starter_kit/src/folders_and_files/metadata.md#tsv-files) for more information.

See also this bids tool to import and export a `participants.tsv` file:
[bids-matlab-tools](https://github.com/sccn/bids-matlab-tools/blob/master/bids_spreadsheet2participants.m)

## How to specify the micro sign in MATLAB?

BIDS requires physical units to be specified according to the SI unit symbol and
possibly prefix symbol (for example: mV, μV for milliVolt and microVolt).

The symbol used to indicate `µ` has unicode U+00B5, which is in MATLAB:

```matlab
char(181)
```

or

```matlab
native2unicode(181, 'latin1')
```

## I had to split the testing of one of my participants across 2 days, should I use 2 different session folders to organize the data of that participant?

(faq_session)=

No. The `session` level in the BIDS folder hierarchy can be used to group data
that go "logically" together: this means that you can put in the same `session` folder
data that were acquired on different days,
but that are "linked" to one another in a way that make sense to how you want to organize your data.

If you want to keep track of what data was acquired when you can use the
[`scans.tsv` files](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#scans-file).

For some examples, see this
[issue in the bids-starter kit](https://github.com/bids-standard/bids-starter-kit/issues/193).

If you deal with EEG data, you may want to read this
[comment in another issue](https://github.com/bids-standard/bids-starter-kit/issues/265#issuecomment-1082240834)
as well before considering combining recordings acquired on different occasions
within the same `session` folder.

## I only have nifti files and no dicom. Can I still create a BIDS dataset?

In theory yes, but it is possible that you will be missing some metadata that is required by the BIDS specification.

A couple of BIDS converters can work with nifti files:

-   bidscoin
-   bidsme
-   explore asl
-   data2bids

See the list of converters [here](https://bids.neuroimaging.io/benefits.html#mri-and-pet-converters).

**However**...

-   They may not work with the datatype you have
    (for example not sure that explore ASL can deal with `func` data).

-   Many of them still expect that you have a json side car for each nifti
    (BIDSme for example - from the top of my head).

-   They may expect a certain input structure to work efficiently,
    so we may have to move files around.

<!-- Not played with it but, from the README, data2bids sounds pretty flexible
but may require you to play with python and json files for configuration.

https://github.com/SIMEXP/Data2Bids -->

### Regarding the "missing" JSON files

Depending on the datatype you are dealing with this can be more of less annoying.

2 examples:

For the most typical `anat` files,
they don't have any REQUIRED metadata,
so you could have just the data files without any accompanying JSON.
If you get into more exotic anat files (like for quantitative MRI)
then this may become a problem.

For `func` files you only need `TaskName` and `RepetitionTime`
and the former you can decide what it is and the latter should be in the Nifti header.
So you should be OKish there too.

Obviously you will be missing some metadata that would be required
for some type of preprocessing (like slice timing info).

### Tips

-   If you have the PDF with the details of acquisition sequence or a method section
    from a paper with that data, you could "recover" some extra metadata.

-   SPM dicom import tool usually leaves a couple of metadata
    in a description field of the nifti header.
    That can be a problem for data anonymisation but may help you in your case
    if this is the tool that was used.
    See an example [here](https://github.com/PeerHerholz/BIDSonym/issues/41#issue-768636869).

-   If you have to script tings manually rather than using a converter
    remember to use pybids
    [path construction tools](https://bids-standard.github.io/pybids/examples/pybids_tutorial.html#path-construction)
    or the
    [bids matlab equivalent](https://github.com/bids-standard/bids-matlab/blob/master/examples/03_BIDS-Matlab_filenames_and_metadata.ipynb) to make your life easier when constructing bids valid filenames.

## Is there a machine readable version of the BIDS specification?

Yes. The BIDS specification exist as a schema.
The BIDS schema is a machine readable representation of the BIDS Standard.
It is (by and large) the BIDS Specification, but written in a declarative form.

The BIDS schema is available in two machine readable formats:

-   as a set of [YAML](https://en.wikipedia.org/wiki/YAML) files in the [BIDS specification repository](https://github.com/bids-standard/bids-specification/src/schema)
-   as a [single json file](https://bids-specification.readthedocs.io/en/stable/schema.json)

A light-weight introduction to the schema can be found [here](https://bids-extensions.readthedocs.io/en/latest/schema/).

A full description of the schema can be found on this [website](https://bidsschematools.readthedocs.io/en/latest/?badge=latest)
where you will also find the documentation for the python package
to interact with the schema, [bidsschematools](https://pypi.org/project/bidsschematools/).

## Is your data type not covered in the current BIDS specification?

BIDS extensions proposals [(BEPs)](https://bids.neuroimaging.io/get_involved.html#extending-the-bids-specification)
aim to extend the BIDS specification to new data types.
A list of extensions proposals can be found below.

Guidelines for contributing to these extensions or starting your own can be found
in the [BIDS Extension Proposals Guide](https://bids-extensions.readthedocs.io/en/latest/).

If only part of your data is covered under BIDS,
an option to allow additional files
currently not covered in BIDS to pass the validator is
the [.bidsignore](https://github.com/bids-standard/bids-validator/blob/master/bids-validator/README.md) file,
which works just like [.gitignore](https://git-scm.com/docs/gitignore).
It allows you to list all the files (or directories, with wildcards)
that are not BIDS compliant and should be ignored by the validator.
Of course you should still try to adhere to upcoming BEPs
and the general BIDS philosophy for file names and metadata where possible,
but this gives a little extra flexibility.

<!-- TEMPLATE STARTS -->

### raw

-   BEP004: [Susceptibility Weighted Imaging (SWI)](https://bids.neuroimaging.io/bep004)
-   BEP020: [Eye Tracking including Gaze Position and Pupil Size](https://bids.neuroimaging.io/bep020)
-   BEP022: [Magnetic Resonance Spectroscopy (MRS)](https://bids.neuroimaging.io/bep022)
-   BEP024: [Computed Tomography scan (CT)](https://bids.neuroimaging.io/bep024)
-   BEP026: [Microelectrode Recordings](https://bids.neuroimaging.io/bep026)
-   BEP032: [Animal electrophysiology](https://bids.neuroimaging.io/bep032)
-   BEP033: [Advanced Diffusion Weighted Imaging (aDWI)](https://bids.neuroimaging.io/bep033)
-   BEP036: [Phenotypic Data Guidelines](https://bids.neuroimaging.io/bep036)
-   BEP037: [Non-Invasive Brain Stimulation (NIBS)](https://bids.neuroimaging.io/bep037)
-   BEP038: [Atlases](https://bids.neuroimaging.io/bep038)
-   BEP039: [Dimensionality reduction-based networks](https://bids.neuroimaging.io/bep039)
-   BEP040: [Functional Ultrasound (fUS)](https://bids.neuroimaging.io/bep040)

### derivative

-   BEP011: [Structural preprocessing derivatives](https://bids.neuroimaging.io/bep011)
-   BEP012: [Functional preprocessing derivatives](https://bids.neuroimaging.io/bep012)
-   BEP014: [Affine transformations and nonlinear field warps](https://bids.neuroimaging.io/bep014)
-   BEP016: [Diffusion weighted imaging derivatives](https://bids.neuroimaging.io/bep016)
-   BEP017: [Generic BIDS connectivity data schema](https://bids.neuroimaging.io/bep017)
-   BEP021: [Common Electrophysiological Derivatives](https://bids.neuroimaging.io/bep021)
-   BEP023: [PET Preprocessing derivatives](https://bids.neuroimaging.io/bep023)
-   BEP034: [Computational modeling](https://bids.neuroimaging.io/bep034)
-   BEP035: [Modular extensions for individual participant data mega-analyses with non-compliant derivatives](https://bids.neuroimaging.io/bep035)
-   BEP041: [Statistical Model Derivatives](https://bids.neuroimaging.io/bep041)

### metadata

-   BEP028: [Provenance](https://bids.neuroimaging.io/bep028)
-   BEP034: [Computational modeling](https://bids.neuroimaging.io/bep034)

## What does [this word] mean?

We're building a glossary to de-jargonise some of the terms you need to know to
work with data in BIDS format. Check it out [here](../starter_kit/src/glossary.md).

## What is a `json` file?

You can find more information about `json` (and `tsv`) files in the
[Metadata-file-formats](../starter_kit/src/folders_and_files/metadata.md#json) page.

## What liense should I choose for my dataset?

If you want to know more about what license to choose for your dataset,
see the [turing way](https://the-turing-way.netlify.app/reproducible-research/licensing/licensing-data.html#data-licenses)
page dedicated to this topic.

If you plan to put your dataset on [openneuro](https://openneuro.org/),
you should use a CC0 or a PDDL license as explained in their [FAQ](https://openneuro.org/faq).

<hr>

Generated by [FAQtory](https://github.com/willmcgugan/faqtory)

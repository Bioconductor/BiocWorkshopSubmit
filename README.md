
# BiocWorkshopSubmit

## Introduction & Installation

The `BiocWorkshopSubmit` package is a Shiny app that allows users to submit
Bioconductor workshops to the [Bioconductor Workshop][1] website via the
[workshop contributions repository on GitHub][2]. The app is designed to be
used as a standalone local app with a particular setup.

The package is only available on GitHub and can be installed with the
following command:

``` r
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("Bioconductor/BiocWorkshopSubmit")
```

[1]: https://workshop.bioconductor.org
[2]: https://github.com/Bioconductor/workshop-contributions

## Workshop Setup

Workshop submitters should model their workshop as packages. There are a
few key files that are required for a smooth workshop experience. These
include a `DESCRIPTION` file, a `Dockerfile`, and vignettes. Note that
the `Dockerfile` will be provided by the [BuildABiocWorkshop][3] template.

[3]: https://github.com/Bioconductor/BuildABiocWorkshop

### `DESCRIPTION` file 

The `DESCRIPTION` file will indicate the packages and the version of R needed
for the workshop. It should also contain some key information about the
workshop, such as the title, description, and URL to the workshop repository.

- Title: A title for the workshop
- Description: A short description of the workshop
- URL: A URL to the workshop repository from either GHCR or Docker Hub

Note. The `URL` can point to either a container hosted by Docker (`docker.io`)
or by the GitHub Container Registry (`ghcr.io`), e.g.,
`ghcr.io/organization/repository`. The `URL` can also be seen at the built
container page on GitHub, e.g.,
<https://github.com/organization/repository/pkgs/container/repository>
where `organization` and `repository` are placeholders for the organization and
repository / container names, respectively.

### `Dockerfile`

The [BuildABiocWorkshop][1] is the recommended way to add a template
`Dockerfile` to a workshop. The `Dockerfile`, in conjunction with the provided
GitHub Actions (GHA), is used to build the container and host it on the GitHub
Container Registry (as set by the `REGISTRY` environment variable in the GHA).

Note. Once the container is built, the workshop should be tested locally to
ensure that it works as expected. The workshop should be tested, preferably on
the latest release version Bioconductor and the appropriate version of R.

### Vignettes

The workshop should contain vignettes that will be used as the workshop
content. As with package vignettes, the vignettes should be placed in the
`vignettes` directory of the workshop repository.

## Creating a GH Issue

The app will create a GitHub issue in the the `workshop-contributions`
repository. Submitters should generate a fine-grained personal access
token via GitHub <https://github.com/settings/tokens>. The token should
have the `public_repo` scope. The token should be saved with
`gitcreds::gitcreds_set()`. Once the token is saved, the app will use it
to create the issue.

## `BiocWorkshopSubmit` Usage

The app can be launched with the following command:

``` r
BiocWorkshopSubmit::BiocWorkshopSubmit()
```

<figure>
<img
src="https://raw.githubusercontent.com/Bioconductor/BiocWorkshopSubmit/devel/vignettes/BiocWorkshopSubmit_interface.png"
alt="BiocWorkshopSubmit Interface" />
<figcaption aria-hidden="true">BiocWorkshopSubmit Interface</figcaption>
</figure>

The app will prompt the presenter to submit the following information:

- GitHub repository: The GitHub repository where the workshop contents
  are hosted.
- Workshop ID: A unique identifier for the workshop, e.g.,
  `tidybioc2023`.
- Section: The section that corresponds to the group of workshops for a
  particular event (if any; e.g., `Bioc2023`).
- Start File: The file that should be open when the user logs in to the
  workshop instance.
- Container Tag: The tag for the container that should be used for the
  workshop (typically, `latest`).

The workshop presenter will be able to auto-populate the fields in the
app by clicking the `Populate` button. The app will use the
`DESCRIPTION` file on GitHub to update the fields in the app.

### Additional Information

<figure>
<img
src="https://raw.githubusercontent.com/Bioconductor/BiocWorkshopSubmit/devel/vignettes/AdditionalInformation.png"
alt="Additional Information" />
<figcaption aria-hidden="true">Additional Information</figcaption>
</figure>

Workshop presenters can also provide additional information regarding
the size, start time, and date of the workshop. This information will be
used to allocate resources for the workshop on the Bioconductor Workshop
website.

- Workshop Date: The date interval of the workshop event.
- Workshop Start Time (24h format): The start time of the workshop event
  in Eastern US time.
- Expected Number of Participants: The expected number of participants
  for the workshop.

### Credentials setup

The app uses the `gh` package to submit an API POST request to create the issue
in the [workshop contributions repository][2]. It is recommended to add a
fine-grained personal access GitHub token to the `gitcreds` package.
The token should have the `public_repo` scope. The token can be added with the
following command:

``` r
gitcreds::gitcreds_set()
```

It is also recommended to use services such as `keychain` on MacOS and
`seahorse` / `Passwords and Keys` on Ubuntu for encrypted key storage.

### Submitting the Workshop

After the details of the workshop have been entered, the submitter can click on
the `Render` button to generate the text needed for the workshop issue
submission at [contributions repository][2]. The text will be displayed on the
right-hand side of the app. If all the details look correct, the workshop
presenter can click the red `Create Issue` button to submit the workshop. The
app will then create a GitHub issue in the `workshop-contributions` repository
with the details of the workshop. Please monitor the issue for any comments from
the Bioconductor team.

Thank you for your contribution to the Bioconductor Workshop website!

## Example Workshops

The following workshops have been submitted to the Bioconductor
Workshop:

- MultiAssayWorkshop: <https://github.com/waldronlab/MultiAssayWorkshop>
- ISMB.OSCA: <https://github.com/Bioconductor/ISMB.OSCA>

Feel free to refer to these workshops as examples of how to structure
the workshop package for submission to the Bioconductor Workshop
website.

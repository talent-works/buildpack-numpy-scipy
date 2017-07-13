# buildpack-numpy-scipy
## The problem
Heroku does not [support](https://devcenter.heroku.com/articles/python-c-deps) obscure dependencies for scipy.

We have been using a [custom buildpack for python](https://github.com/andrewychoi/heroku-buildpack-scipy) which is very outdated. We want to move first to python 2.7.13 and then to python 3.

## The solution
Create a heroku environment with the right C dependencies, build numpy and scipy there, tar up the libraries and install them on the python backend.

### Building on a temporary heroku environment
Use the [buildpack-apt](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-apt) with a custom [Aptfile](https://s3-us-west-2.amazonaws.com/talentworks-data/curated/production/npscipy-binaries/20170712/Aptfile) and a placeholder requirements.txt, login and run the [commands](https://s3-us-west-2.amazonaws.com/talentworks-data/curated/production/npscipy-binaries/20170712/commands) to place the tar files into S3.

### Using the tar files instead of pip installing numpy/scipy
Use [buildpack-numpy-scipy](https://github.com/talent-works/buildpack-numpy-scipy) *after* the supported heroku buildpack for python. Break up all the dependencies in the requirements file which need numpy into a new file called numpy-scipy-requirements.txt

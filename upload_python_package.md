# How to upload your python package to PyPi

The majority of this file is mostly based on the article found here: https://medium.com/@joel.barmettler/how-to-upload-your-python-package-to-pypi-65edc5fe9c56 (last accessed on June 2, 2021. The article was written orginally by joelbarmettlerUZH. Detailed instructions are available through the Python documentation page here: https://packaging.python.org/guides/distributing-packages-using-setuptools/. I modified the original article based on my own experience.

The PyPi package index is one of the properties that makes python so powerfull: With just a simple command, you get access to thousands of cool libraries, ready for you to use. In this short introduction, I am going to explain how you can upload upload your own code to PyPi and let others profit from your inventions. On the following pages, This file will walk you through the following key steps to upload your codes/packages to PyPi (https://pypi.org):

* Create a python package
* Create the files PyPi needs
* Create a PyPi account
* Upload your package to github.com
* Upload your package to PyPi
* Install your own package using pip
* Change your package

## Create a python package

To create a package, create a folder that is named exactly how you want your package to be named. Place all the files and classes that you want to ship into this folder.

Now, create a file called __init__.py (two underscores). The __init__.py file is used to mark which classes you want the user to access through the package interface. Open the file with your text editor of choice. In this file, you write nothing but import statements that have the following schema:

```Python
from packagename.Filename import Classname
```

Note that you can actually leave the __init__.py file empty. For me, I copied and pasted the license text in it as comments. 

Your package folder should include the following:

* mylib: directory to put your real python codes. This is your real package.
* other files or folders as metadata. See the next section on these files.

## Create the files PyPi needs

PyPi needs three files in order to work:

setup.py

setup.cfg

LICENSE.txt

README.md (optinal but highly recommended)

Place all these files outside of your package folder.

### setup.py
You can get a long example of a setup.py from here: https://github.com/pypa/sampleproject/blob/main/setup.py. The following is an example of a setup.py file. In this example, I used a variable to store the version string and then call this variable in the download_url. Luckily, the release URL for the tar file by GitHub is simply created following the version string. **Make sure you create your GitHub release following the same version string.**

```Python
from setuptools import setup, find_packages
import pathlib

here = pathlib.Path(__file__).parent.resolve()

# Get the long description from the README file
long_description = (here / 'description.md').read_text(encoding='utf-8')

# Arguments marked as "Required" below must be included for upload to PyPI.
# Fields marked as "Optional" may be commented out.
version='0.5.9'
setup(
    name='seisgo',
    version=version,
    description='A ready-to-go Python toolbox for seismic data analysis',
    author='Xiaotao Yang',
    author_email='stcyang@gmail.com',
    maintainer='Xiaotao Yang',
    maintainer_email='stcyang@gmail.com',
    download_url='https://github.com/xtyangpsp/SeisGo/archive/refs/tags/v'+version+'.tar.gz',

    # This is an optional longer description of your project that represents
    # the body of text which users will see when they visit PyPI.
    #
    # Often, this is the same as your README, so you can just read it in from
    # that file directly (as we have already done above)
    #
    # This field corresponds to the "Description" metadata field:
    # https://packaging.python.org/specifications/core-metadata/#description-optional
    long_description=long_description,  # Optional

    # Denotes that our long_description is in Markdown; valid values are
    # text/plain, text/x-rst, and text/markdown
    #
    # Optional if long_description is written in reStructuredText (rst) but
    # required for plain-text or Markdown; if unspecified, "applications should
    # attempt to render [the long_description] as text/x-rst; charset=UTF-8 and
    # fall back to text/plain if it is not valid rst" (see link below)
    #
    # This field corresponds to the "Description-Content-Type" metadata field:
    # https://packaging.python.org/specifications/core-metadata/#description-content-type-optional
    long_description_content_type='text/markdown',  # Optional (see note above)

    # This should be a valid link to your project's main homepage.
    url='https://github.com/xtyangpsp/SeisGo',  # Optional

    # Classifiers help users find your project by categorizing it.
    #
    # For a list of valid classifiers, see https://pypi.org/classifiers/
    classifiers=[  # Optional
        # How mature is this project? Common values are
        #   3 - Alpha
        #   4 - Beta
        #   5 - Production/Stable
        'Development Status :: 4 - Beta',

        # Pick your license as you wish
        'License :: OSI Approved :: MIT License',

        # Specify the Python versions you support here. In particular, ensure
        # that you indicate you support Python 3. These classifiers are *not*
        # checked by 'pip install'. See instead 'python_requires' below.
        'Programming Language :: Python :: 3.7'
    ],

    # This field adds keywords for your project which will appear on the
    # project page. What does your project relate to?
    #
    # Note that this is a list of additional keywords, separated
    # by commas, to be used to assist searching for the distribution in a
    # larger catalog.
    keywords='seismology, seismic data analysis, seismic toolbox',  # Optional

    # When your source code is in a subdirectory under the project root, e.g.
    # `src/`, it is necessary to specify the `package_dir` argument.
    #package_dir={'': 'src'},  # Optional

    # You can just specify package directories manually here if your project is
    # simple. Or you can use find_packages().
    #
    # Alternatively, if you just want to distribute a single Python file, use
    # the `py_modules` argument instead as follows, which will expect a file
    # called `my_module.py` to exist:
    #
    #   py_modules=["my_module"],
    #
    #packages=find_packages(where='src'),  # Required

    packages=['seisgo'],
    include_package_data = True,
    package_data={"":["data","figs","notebooks"]},
    # Specify which Python versions you support. In contrast to the
    # 'Programming Language' classifiers above, 'pip install' will check this
    # and refuse to install the project if the version does not match. See
    # https://packaging.python.org/guides/distributing-packages-using-setuptools/#python-requires
    python_requires='>=3.6',

    # This field lists other packages that your project depends on to run.
    # Any package you put here will be installed by pip when your project is
    # installed, so they must be valid existing projects.
    #
    # For an analysis of "install_requires" vs pip's requirements files see:
    # https://packaging.python.org/en/latest/requirements.html
    install_requires=['numpy',
                        'scipy',
                        'pandas',
                        'obspy',
                        'pyasdf',
                        'numba',
                        'pycwt'
        ]
)


```

## Create a PyPi account
You can register yourself for a PyPi account here: https://pypi.org/account/register/. Remember your username (not the Name, not the E-Mail Address) and your password, you will need it later for the upload process.

## Upload your package to github.com
Create a github repo including all the files we are going to create in a second, as well as your package folder. Name the repo exactly as the package. This step needs knowledge beyond the scope of this file. I will skip details on how to upload/create a GitHub repository.

You will need to create a release with the same version number as in your **setup.py** file. Go the release, copy the link URL to the tar.gz file and paste it to the **setup.py** download URL.

## Upload your package to PyPi
Now, the final step has come: uploading your project to PyPi. First, open the command prompt and navigate into your the folder where you have all your files and your package located.

Now, we create a source distribution with the following command:

```Python
python setup.py sdist
```

We will need twine for the upload process, so first install twine via pip:

```Python
pip install twine
```

Then, run the following command:

```Python
twine upload dist/*
```

You will be asked to provide your username and password. Provide the credentials you used to register to PyPi earlier. The file output line, if successful, will tell you where to check your package. Such as: View at: https://pypi.org/project/seisgo/0.5.9/ . **Congratulations, your package is now uploaded!** 

## Install your own package using pip
Okay, now let’s test this out. Open your console and type the following command:

```Python

pip install YOURPACKAGENAME
```

## Change your package
If you maintain your package well, you will need to change the source code form time to time. This is easy. Simply upload your new code to github, create a new release, then adapt the setup.py file (new download_url — according to your new release tag, new version), then run the setup.py and the twin command again (navigate to your folder first!)

```Python
python setup.py sdist
twine upload dist/*
```

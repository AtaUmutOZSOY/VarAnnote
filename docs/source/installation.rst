Installation Guide
==================

VarAnnote can be installed using pip, conda, or from source. This guide covers all installation methods and system requirements.

System Requirements
-------------------

**Python Version**
  * Python 3.8 or higher
  * Recommended: Python 3.9+

**Operating Systems**
  * Linux (Ubuntu 18.04+, CentOS 7+)
  * macOS (10.14+)
  * Windows (10+)

**Memory Requirements**
  * Minimum: 2GB RAM
  * Recommended: 4GB+ RAM for large datasets
  * For 1000+ variants: 8GB+ RAM recommended

**Disk Space**
  * Base installation: ~100MB
  * Cache directory: 1-10GB (depending on usage)
  * Temporary files: 100MB-1GB per analysis

Installation Methods
--------------------

Method 1: PyPI Installation (Recommended)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to install VarAnnote is using pip::

    pip install varannote

To install with all optional dependencies::

    pip install varannote[all]

To install the latest development version::

    pip install git+https://github.com/varannote/varannote.git

Method 2: Conda Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

VarAnnote is available on conda-forge::

    conda install -c conda-forge varannote

Or using mamba (faster)::

    mamba install -c conda-forge varannote

Method 3: From Source
~~~~~~~~~~~~~~~~~~~~~

For development or the latest features::

    # Clone the repository
    git clone https://github.com/varannote/varannote.git
    cd varannote
    
    # Install in development mode
    pip install -e .
    
    # Or install normally
    pip install .

Virtual Environment Setup
-------------------------

It's recommended to use a virtual environment::

    # Using venv
    python -m venv varannote-env
    source varannote-env/bin/activate  # Linux/macOS
    # varannote-env\Scripts\activate   # Windows
    
    pip install varannote

    # Using conda
    conda create -n varannote python=3.9
    conda activate varannote
    conda install -c conda-forge varannote

Dependencies
------------

**Core Dependencies**
  * requests >= 2.25.0
  * pandas >= 1.3.0
  * pyyaml >= 5.4.0
  * aiohttp >= 3.7.0
  * click >= 8.0.0

**Optional Dependencies**
  * matplotlib >= 3.3.0 (for plotting)
  * seaborn >= 0.11.0 (for advanced plots)
  * jupyter >= 1.0.0 (for notebooks)
  * pytest >= 6.0.0 (for testing)

**Development Dependencies**
  * black (code formatting)
  * flake8 (linting)
  * mypy (type checking)
  * sphinx (documentation)

Installation Verification
-------------------------

After installation, verify that VarAnnote is working correctly::

    # Check version
    varannote --version
    
    # Run basic test
    python -c "from varannote import VarAnnote; print('Installation successful!')"
    
    # Run comprehensive test
    varannote test

Configuration Setup
-------------------

After installation, set up your configuration::

    # Create default configuration
    varannote config init
    
    # Edit configuration file
    varannote config edit
    
    # Validate configuration
    varannote config validate

The configuration file will be created at::

    # Linux/macOS
    ~/.varannote/config.yaml
    
    # Windows
    %USERPROFILE%\.varannote\config.yaml

API Keys Setup
--------------

Some databases require API keys. Set them up after installation:

**Environment Variables**::

    export VARANNOTE_COSMIC_API_KEY="your_cosmic_key"
    export VARANNOTE_PHARMGKB_API_KEY="your_pharmgkb_key"

**Configuration File**::

    databases:
      api_keys:
        cosmic: "your_cosmic_key"
        pharmgkb: "your_pharmgkb_key"

**Interactive Setup**::

    varannote config set-api-key cosmic your_cosmic_key
    varannote config set-api-key pharmgkb your_pharmgkb_key

Troubleshooting
---------------

**Common Installation Issues**

*Permission Denied Error*::

    # Use user installation
    pip install --user varannote

*SSL Certificate Error*::

    # Upgrade certificates
    pip install --upgrade certifi
    
    # Or use trusted hosts
    pip install --trusted-host pypi.org --trusted-host pypi.python.org varannote

*Dependency Conflicts*::

    # Create fresh environment
    conda create -n varannote-clean python=3.9
    conda activate varannote-clean
    pip install varannote

*Import Errors*::

    # Check installation
    pip show varannote
    
    # Reinstall if needed
    pip uninstall varannote
    pip install varannote

**Performance Issues**

*Slow Installation*::

    # Use faster index
    pip install -i https://pypi.douban.com/simple/ varannote
    
    # Or use conda
    mamba install -c conda-forge varannote

*Memory Issues During Installation*::

    # Increase pip cache
    pip install --cache-dir /tmp/pip-cache varannote
    
    # Or disable cache
    pip install --no-cache-dir varannote

**Platform-Specific Issues**

*Windows Long Path Issues*::

    # Enable long paths in Windows
    # Run as administrator:
    # New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force

*macOS Permission Issues*::

    # Install with homebrew python
    brew install python
    /usr/local/bin/pip3 install varannote

*Linux Missing Dependencies*::

    # Ubuntu/Debian
    sudo apt-get update
    sudo apt-get install python3-dev python3-pip
    
    # CentOS/RHEL
    sudo yum install python3-devel python3-pip

Upgrading VarAnnote
-------------------

To upgrade to the latest version::

    # Using pip
    pip install --upgrade varannote
    
    # Using conda
    conda update varannote
    
    # Check new version
    varannote --version

To upgrade from a specific version::

    pip install varannote>=1.0.0

Uninstallation
--------------

To completely remove VarAnnote::

    # Uninstall package
    pip uninstall varannote
    
    # Remove configuration and cache
    rm -rf ~/.varannote/  # Linux/macOS
    # rmdir /s %USERPROFILE%\.varannote  # Windows

Docker Installation
-------------------

VarAnnote is also available as a Docker image::

    # Pull the image
    docker pull varannote/varannote:latest
    
    # Run VarAnnote
    docker run -v $(pwd):/data varannote/varannote:latest annotate /data/input.vcf
    
    # Interactive mode
    docker run -it -v $(pwd):/data varannote/varannote:latest bash

Getting Help
------------

If you encounter issues during installation:

1. Check the `troubleshooting section <#troubleshooting>`_ above
2. Search existing `GitHub issues <https://github.com/varannote/varannote/issues>`_
3. Create a new issue with:
   * Your operating system and Python version
   * Complete error message
   * Installation method used
   * Output of ``pip list`` or ``conda list``

**Support Channels**
  * GitHub Issues: https://github.com/varannote/varannote/issues
  * Documentation: https://varannote.readthedocs.io
  * Email: support@varannote.org 
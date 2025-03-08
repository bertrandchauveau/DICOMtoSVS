# DICOMtoSVS
To convert brightfield and singleplex fluorescence DICOM whole slide images into SVS-like TIFF pyramidal images

This work is currently under review at a scientific journal. A link to the published article and to the Windows executable will be displayed in due time.

Pathology Departments are encouraged to use Digital Imaging and Communication in Medicine (DICOM) for their workflow of whole slide images (WSI), like radiologists before them. While this allows for a secure and universal workflow in routine with WSI from scanners of various vendors, as of 2025, DICOM adoption for WSI remains emerging and DICOM is not supported by some web-based platforms dedicated to collaborative diagnosis and research (e.g. TeleSlide, Cytomine) or some python packages dedicated to WSI (e.g., RAPIDS cuCIM).

DICOM is expected to be the future reference WSI format, and as such commercial software will ultimately add support for it. In the meantime, one solution is to convert DICOM WSI into a more common WSI file format. Here is proposed an SVS-like pyramidal TIFF organization. SVS files are actual TIFF files, with no proprietary extensions, and is the WSI file format used by Aperio (Leica Biosystems).

Here is proposed a Python-based solution to convert DICOM WSI into SVS-like TIFF pyramidal images. These slides can then be opened by common software supporting the SVS format (Aperio ImageScope, QuPath, TeleSlide, ...). This conversion mainly relies on the Pydicom package for reading DICOM slides and tifffile for writing the SVS-like file.

The code is provided in 3 ways: 
- a Colab-compatible jupyter notebook for easy testing
- a Python script to be run through Command Line Interface
- a Windows and MacOS executable created using PyInstaller 6.9.0, as such prior coding knowledge is not required
<a target="_blank" href="https://colab.research.google.com/github/bertrandchauveau/DICOMtoSVS/blob/main/DICOM_to_SVS.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

The Python script was tested using a Windows operating system in a conda virtual environment.
The main dependencies used were:
- Pydicom 2.4.4
- imagecodecs 2024.6.1
- tifffile 2024.2.12
- natsort 8.4.0
- numpy 1.26.4
- pillow 10.4.0
- pylibjpeg 2.0.1

The Windows executable was tested on Windows 10 Professional 22H2 and Windows 11 Professional 23H2. The only required dependency is Microsoft Visual C++ Redistributable, available at https://learn.microsoft.com/fr-fr/cpp/windows/latest-supported-vc-redist?view=msvc-170.
The MacOS executable was tested on MacOS Sequoia 15.3.1.

Using a 13th Gen Intel(R) Core(TM) i7-13700 with 16Gb of RAM, the mean time to convert a 1Gb WSI is about 18 seconds. Label and macro images, when present in the original DICOM file, can either be removed or retained during conversion. Optionally, additional DICOM tags can be embedded in the converted file in a custom TIFF tag (65000). Moreover, WSI anonymization can be performed during conversion, by renaming the WSI, and by removing label and macro images, together with optional metadata.

The solution has been tested in these situations:
- Single plane brightfield images, DICOM WSI originating from:
  
    => Leica (Aperio GT450 DX)
  
    => 3DHistech (Pannoramic scan P150, P1000)
  
    => Roche (DP600 and DP200)
  
    => Hamamatsu (S360)
  
    => Olympus/Evident (VS200 and DX VS M1)
  
- Single plex immunofluorescence images:

    => 3DHistech (Pannoramic scan P150)

The arguments, to be defined through Tkinter user interface are:
- path_to_folder : string, path to the folder containing the DICOM files
- is_zipped : boolean, if the original files are zipped
- label : boolean, add label image if exists
- macro : boolean, add macro image if exists
- anonymize : boolean, whether to anonymize the slide during conversion
- add_DICOM_tags : boolean, whether to add DICOM tags into the file metadata (stored as a dictionary in a custom TIFF tag)

## Usage (Python script):
- considering a Python environment with the required dependencies:
  
```python /path_to/DICOMtoSVS.py```

## Installation instructions and usage (Windows or MacOS executable):
- end-users must seek the validation of their information technology service management before using the application on an institutional device and only use DICOM originating from a trusted source
- download the DICOMtoSVS.zip file at: pending
- decompress the file in your local disk, ending up with a DICOMtoSVS folder containing a "DICOMtoSVS.exe" file and a "_internal" folder, containing required files to run the executable. Do not separate the "_internal" folder from the exe file. 
- optional: create a desktop shortcut of the .exe file (right-clik, create shortcut)
- when running the .exe file for the first time, Windows will display a warning message "unknown publisher". This is an expected behavior from Windows.
- running the .exe file will launch a command prompt and, a few seconds later, another window to select the arguments for the WSI conversion. You should point out the folder where the native DICOM files are (.../native_folder). It is not expected that the selected folder contains other files or folder types.
- The command prompt is automatically closed at the end of the script (at least for Windows). Converted files are stored at .../native_folder_ouput


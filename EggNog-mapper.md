https://github.com/eggnogdb/eggnog-mapper/wiki/eggNOG-mapper-v2.1.5-to-v2.1.6#Conda_bioconda_channel_version

	
	(base) foodlabui-iMac:~ foodlab$ pip install eggnog-mapper
	
.
	
	Requirement already satisfied: eggnog-mapper in /opt/anaconda3/lib/python3.8/site-packages (2.1.3)
	Collecting biopython==1.76
	  Downloading biopython-1.76-cp38-cp38-macosx_10_9_x86_64.whl (2.3 MB)
	     |████████████████████████████████| 2.3 MB 8.0 MB/s 
	Collecting psutil==5.7.0
	  Downloading psutil-5.7.0.tar.gz (449 kB)
	     |████████████████████████████████| 449 kB 46.0 MB/s 
	Requirement already satisfied: numpy in /opt/anaconda3/lib/python3.8/site-packages (from biopython==1.76->eggnog-mapper) (1.20.3)
	Building wheels for collected packages: psutil
	  Building wheel for psutil (setup.py) ... done
	  Created wheel for psutil: filename=psutil-5.7.0-cp38-cp38-macosx_10_9_x86_64.whl size=227419 sha256=81c723e9e24d555e308a30ad29b673363dab27d5470f8217fd3c398980e5d8aa
	  Stored in directory: /Users/FOODLAB/Library/Caches/pip/wheels/90/c9/b6/04665702b01dbd9ee92a05e834b627948ed01cdd482e6a78e1
	Successfully built psutil
	Installing collected packages: psutil, biopython
	  Attempting uninstall: psutil
	    Found existing installation: psutil 5.8.0
	    Uninstalling psutil-5.8.0:
	      Successfully uninstalled psutil-5.8.0
	  Attempting uninstall: biopython
	    Found existing installation: biopython 1.78
	    Uninstalling biopython-1.78:
	      Successfully uninstalled biopython-1.78
	ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
	spyder 4.2.5 requires pyqt5<5.13, which is not installed.
	spyder 4.2.5 requires pyqtwebengine<5.13, which is not installed.
	Successfully installed biopython-1.76 psutil-5.7.0
	(base) foodlabui-iMac:~ foodlab$ 







===========================
**Combine GAFs on CyVerse**
===========================

**Accessing GOanna in the Discovery Environment**
=================================================

1. `Create an account on CyVerse <user.cyverse.org>`_ (free). The user guide can be found `here <https://learning.cyverse.org/>`_.
2. Open the CyVerse Discovery Environment (DE) and login with your CyVerse credentials.

4. There are several ways to access the combine_GAFs app:

- Use the `direct link <https://de.cyverse.org/apps/de/f707a7a4-4c3c-11ee-bba8-008cfa5ae621>`_.
- Search for 'combine_GAFs in the search bar at the top of the ‘apps’ tab.
- Follow the AgBase collection (collections tab on left side of DE)

|find_combine_gafs|


**Using the Combine_GAFs App**
------------------------------
**Step 1. Analysis Info**
^^^^^^^^^^^^^^^^^^^^^^^^^

|combine_gafs|


**Analysis Name: Combine_GAFs_analysis1:**
This menu is used to name the job you will run so that you can find it later.
Analysis Name: The default name is "Combine_GAFs_analysis1". We recommend changing the 'analysis1' portion of this to reflect the data you are running.

**Comments:**
(Optional) You can add additional information in the comments section to distinguish your analyses further.

**Select output folder:**
This is where your results will be placed. The default (recommended) is your 'analyses' folder.

**Step 2. Parameters**
^^^^^^^^^^^^^^^^^^^^^^

**GOanna GAF Output File:** This is the GAF file generated by a GOanna analysis.

**InterProScan XML Parser GAF Output File:** This is the GAF output file generated by an InterProScan XML Parser analysis. InterProScan itself does not produce this file, though some IntperProScan apps include this analysis. If it is missing from your InterProScan output you can generate it using the InterProScan XML Parser app.

**Output**

**Output File Basename:** This will be the prefix for your output file (a .tsv extension will be added).

**Step3. Adavanced Settings (optional)**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This page allows you specifiy compute requirements for your analysis (e.g. more memory if your analysis is particularly large). You should be able to leave the defaults for most analyses.

**Step4. Review and Launch**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This will display all of the parameters you have set (other than default). Missing information that is required will displayed in red. Make sure you are happy with your choices and then clicke the 'launch' button at the bottom.

If your analysis fails please check the 'condor_stderr' file in the analysis output 'logs' folder. If that doesn't clarify the problem contact us at agbase@email.arizona.edu or support@cyverse.org.

.. |find_combine_gafs| image:: ../img/find_combine_gafs.png


.. |combine_gafs| image:: ../img/combine_gafs.png


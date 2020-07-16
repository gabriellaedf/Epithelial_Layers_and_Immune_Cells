# Epithelial_Layers_and_Immune_Cells

CellProfiler (v3.1.8) pipelines were here used to define four epithelial layers within images of ectocervical tissue epithelium, immunofluorescent stained for E-cadherin and CD4. The integrity of the E-cadherin net structure was determined. The frequency of CD4 positive immune cells was also determined as well as the distance from the luminal surface of the epithelium to the CD4 cells, and the distribution of CD4 cells within each of the four epithelial layers.

Starting images: 
To run the described pipelines a raw RGB image of the epithelial region of interest (ROI) exported from Pannoramic Viewer on a black background was used (Ex1.tif). Different ways of defining the line following the apical border of the epithelium (apical line), the basal border of the epithelium (basal line) and the apical border of the E-cadherin staining (E-cadherin line) can be used. At the end of this document the four pipelines (pipelines I-IV) and manual steps used to define these three lines in this study are described. Example images are provided to enable the reader to directly start with pipeline 1-6 below.


PIPELINES 1-6:
Starting images are: 
Ex1.tif (raw) 
Ex1_neur.tif (neuriteness filtered image)
Ex1_A.png (apical line) (Alt. from pipe II below)
Ex1_B.png (basal line) (Alt. from pipe II below)
Ex1_E.png (e-cadherin line) (Alt. from pipe IV below)

Pipeline 1: 1_SetEcadherinNetTreshold.cppipe
Defines the E-cadherin net structure based on the output from the neuriteness filtered raw image. 
Import: Ex1_neur.tif (neuriteness filtered image)
Export: Ex1_neurnet.png

Pipeline2: 2_ExportEpithelialLayers.cppipe
Identifies the four epithelial layers and exports these as images. Measures the average thickness of each layers as well as the total epithelial thickness. 
Import: 
Ex1_A.png (apical line) (Alt. from pipe II)
Ex1_B.png (basal line) (Alt. from pipe II)
Ex1_E.png (e-cadherin line) (Alt. from pipe IV)
Ex1.tif (raw)
From pipe 1: Ex1_neurnet.png
Export: 
Ex1_sup.png (Superficial layer image)
Ex1_upper.png (Upper Intermediate layer image)
Ex1_lower.png (Lower intermediate layer image)
Ex1_para.png (Parabasal layer image)
Thick_image.csv (Excelfile with thickness measurements)

Pipeline 3: 3_Measure_net_intensity_in3layers.cppipe
Measures the mean fluorescent intensity (MFI) of the E-cadherin net structure in each of the three epithelial layers (containing net) as well as the whole net structure, and percentage of net area per epithelial layer area.
Import: 
Ex1.tif (raw)
From pipe 1: Ex1_neurnet.png
From pipe 2: Ex1_upper.png, Ex1_lower.png, Ex1_para.png
Export: 
NetIntin3layers_Image.csv (Excelfile with mean fluorescent intensity (MFI) measurements and percentage area of the E-cadherin net structure in each of the three epithelial layers (containing net).)

Pipeline 4: 4_SegmentCD4positivearea.cppipe
Segments CD4 positive areas. Contains a module to manually remove false positive objects, module #17 EditObjectsManually. When this window opens, Click reverse selection (to un-select all the segmented CD4 positive areas), Click F (to freehand draw) and draw around the false positive objects (background) to create a new object covering the background to be removed (For PC users, press down the left mouse and draw, release when done), Click done. This pipes measures the frequency, i.e. the percentage of positive CD4 area per total epithelial (ROI) area. 
Import: 
Ex1.tif (raw)
Export: 
Ex1_redobjects.png (CD4 positive area image)
CD4_Image. csv (Excelfile with CD4 frequency measures)

Pipeline 5: 5_CD4_distance_to_apical_line.cppipe
Measures the mean distance from the CD4 positive area to the apical line
Import: 
Ex1_A.png (apical line)
From pipe 4: Ex1_redobjects.png
Export: 
DistCD4_Image.csv (Excelfile with distance measurements from CD4 positive area to apical line.)

Pipeline 6: 6_MeasureCD4_in3layers.cppipe
Measure the distribution of CD4 positive cells in the three epithelial layer that contain CD4 cells. I.e. the proportion of CD4 positive area in each layer out of total CD4 positive area in the whole ROI.
Import: 
From pipe 2: Ex1_upper.png, Ex1_lower.png, Ex1_para.png
From pipe 4: Ex1_redobjects.png
Export: 
RedArea_Image.csv (Excelfile with distance measurements from CD4 positive area to apical line.)



PIPELINES I-IV:
Pipelines describing identification of the line following the apical border of the epithelium (apical line), the basal border of the epithelium (basal line) and the apical border of the E-cadherin staining (E-cadherin line).

Pipeline_I: I_Save_ROI_outlines.cppipe
Identifies a one-pixel thick outline of the region of interest (ROI) exported from Pannoramic Viewer on a black background. 
Import: Ex1.tif (raw)
Export: Ex1_roi.png
Prior to running pipeline II: Make a copy of the Ex1_roi.png image, open in Preview, draw a black 4-pixel line across each of the four corners, to create an image with four separate white lines that separates the apical and basal line as well as the two perpendicular sides of the ROI. Save this edited image as Ex1_roiedit.png.

Pipeline II: II_Identify_A_and_B_line.cppipe
Identifies the apical and the basal line. 
To save the apical line as a separate image and the basal line as a separate image, the following two EditObjectsManually modules was used; 
#6 EditObjectsManually, will identify the apical line: When the window appears, Click reverse selection (all lines will appear dotted). Click on the apical line (this line will become a full line again), Click done. (It is possible to zoom in, but don’t forget to un-click the zoom button to be able to click on the line in the image)
#7 EditObjectsManually, will identify the basal line: When the window appears, Click reverse selection (all lines will appear dotted). Click on the basal line (this line will become a full line again), Click done. 
Import: Ex1_roiedit.png, Ex1.tif (raw)
Export: Ex1_A.png, Ex1_B.png

Pipeline III: III_EnhanceGreen
To facilitate the manual step of drawing the line where the E-cadherin staining ends, this pipeline will save an image with the green channel enhanced. 
Import: Ex1.tif
Export: Ex1_enhanced.png
Prior to running Pipeline IV. Open the Ex1_enhanced.png image in Preview, draw a one-pixel thick white line following the apical end of the E-cadherin staining. Make sure to draw the line all the way across the side lines of the ROI. Save this image as Ex1_enhancededit.png

Pipeline IV: IV_Find_white_line_in_enhancedgreen.cppipe
This pipeline identifies the white line in the enhanced green image. 
Import: edited Ex1_enhancededit.png, Ex1.tif (raw)
Export: Ex1_E.png (E-cadherin line)





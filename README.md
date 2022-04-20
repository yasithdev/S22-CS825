# Antibiotic Drug Resistance in Bacteria
CS 725 / 825 Visualization Project Report  
[Open Notebook](https://observablehq.com/d/f6f919e850bc1d73)

## Introduction

I’m currently collaborating on a project to identify the presence of antibiotic drug resistant bacteria in blood specimens acquired from human subjects. In this collaboration, I apply machine learning techniques on microscopic images of blood specimens to make clinically-relevant predictions. While this is a classic ML problem, visualizing the statistics of collected specimens helps to
(a) understand inherent limitations of the acquired data, and
(b) make informed decisions on the model architecture and future data acquisition.

For this reason, I created an interactive dashboard to visualize different statistics of the microscopic bacteria cell images. It visualizes these statistics at two levels: species level and specimen level (which I explain in the next section). I personally used this dashboard to improve my intuition of the data, and to discover interesting properties that the ML models could leverage.

## Data

The selected dataset (QPM Bacteria) contains information on blood specimens collected from 21 subjects infected with 5 different species of bacteria.
1. Acinetobacter
2. B_subtilis
3. E_coli
4. K_pneumoniae
5. S_aureus

![Example Bacteria Images](/assets/bacteria-images.png)

Of the 21 subjects, 16 were infected with mutated bacteria strains (which are antibiotic drug resistant), while the remaining 5 were infected with bacteria strains in their natural, non-mutated form. The dataset was created using the following steps.

1. Each specimen was imaged through quantitative phase microscopy (QPM) at high resolution. Since blood specimens usually contain a large proportion of non-bacteria cells (e.g., blood cells) compared to bacteria cells (which is what we are interested in), individual bacteria cells were segmented out and labeled by specimen name.
2. Microbial genome analysis was performed on each specimen to extract clinically-relevant properties of the bacterial infection (species / morphology / gram / strain).

**Unfortunately, the bacteria cell images aren’t yet authorized for public use (this image is from a published paper). Therefore, I use statistical features of bacteria cell images (mean / std / count) and their clinically-relevant properties to populate the interactive dashboard.**

## Motivation

While one could rapidly image blood specimens using QPM, performing microbial genome analysis to obtain clinically-relevant properties takes several days. Therefore, if at all possible, being able to reliably obtain clinically-relevant properties from imaging itself, would dramatically speed up the diagnosis of bacterial infection, and potentially save lives. This is what I attempt to explore using the interactive dashboard. In particular, I explore the following questions.
* Q1 - What are the different species and specimens in the dataset?
* Q2 - What types of blood specimens are missing from the dataset?
* Q3 - Can the performance of the machine learning models be explained through the dashboard?

## Visualization

![Screenshot of the Interactive Dashboard](/assets/dashboard-1.png)

The dashboard lets you explore statistics of bacteria cells by choosing different strains and species from the legends on the right. By default, no selection is made, and you could observe all data. The interactivity comes from (1) tooltips and (2) the ability to focus on different strains/species using the two legends. **In particular, selecting items from the legends will highlight the contribution of selected groups in contrast to others.**

### Row 1

![Row 1 of the Interactive Dashboard](/assets/dashboard-row-1.png)

The first row of the interactive dashboard (i.e., species level visualization) shows three column charts for Specimen Count, Cell Count, and Cell Thickness Range. The columns of the first two charts are stacked by strain (i.e., they represent the aggregation of specimen counts and cell counts across both strains). The third chart is faceted (row-wise) by species with a separate column for each strain. The horizontal line in each bar represents the mean cell thickness, and the bar heights represent their extent. The color of each column/stack represents one strain.

![Row 1 of the Interactive Dashboard - Tooltips](/assets/dashboard-row-1-tooltips.png)

Hovering over the different columns (or stacks in the case of stacked column charts) would display a tooltip with additional information about that species.

**(a) Clinically-relevant properties**

1. Species - One of 5 species in the legend
2. Gram Stain (i.e., presence of a cell membrane) - Positive (P), Negative (N)
3. Morphology (i.e., shape) - Elongated (Rod), Round (Spherical)
4. Strain (i.e., antibiotic drug resistance) - Resistant, Non-Resistant

**(b) Statistical features**

1. Cell count (i.e., number of observed bacteria cells)
2. μ (i.e., mean observed cell thickness) and σ (i.e., std of observed cell thickness) of bacteria cells
3. max/min (i.e., max/min observed thickness) of bacteria cells

![Row 1 of the Interactive Dashboard - Selections](/assets/dashboard-row-1-selections.png)

The legend of strain can be used to interactively highlight different strains of bacteria. When a strain is selected, the first two charts display only the specimen counts and cell counts of that strain. The third chart, however, highlights the contribution of that strain, and reduces the opacity of other columns to facilitate comparison.

### Row 2

![Row 2 of the Interactive Dashboard](/assets/dashboard-row-2.png)

The second row of the interactive dashboard (i.e., specimen level visualization) shows two bar charts for Cell count and Cell Thickness Range. Each bar in these charts represents a specimen, and the color of the bar chart represents their species. The legend shown at the end of the row indicates the mapping of colors to species. For example, the blue bars represent specimens of the Acinetobacter species.
Hovering over different bars would display a tooltip with additional information about that specimen.

**(a) Clinically-relevant properties**

1. Species (i.e., which species the specimen belongs to) - One of 5 species in the legend
2. Gram Stain (i.e., presence of a cell membrane) - Positive (P), Negative (N)
3. Morphology (i.e., shape) - Elongated (Rod), Round (Spherical)
4. Strain (i.e., antibiotic drug resistance) - Resistant, Non-Resistant

**(b) Statistical features**

1. Cell count (i.e., number of observed bacteria cells)
2. μ (i.e., mean observed cell thickness) and σ (i.e., std of observed cell thickness) of bacteria cells
3. max/min (i.e., max/min observed thickness) of bacteria cells

The legend of species can be used to interactively traverse different species of bacteria. When a species is selected, the contribution of that species is highlighted in contrast to other species.

![Row 2 of the Interactive Dashboard - Selections](/assets/dashboard-row-2-selections.png)

For instance, the dashboard looks like the above figure when Acinetobacter is selected. Here, the bars representing specimens of that species are highlighted by reducing the opacity of the other bars.

### Combined Interactivity

![Combined Interactivity of the Dashboard (Strain)](/assets/dashboard-interactivity-1.png)

This dashboard not only facilitates individual analysis at strain and species levels, but also allows combinations of them. In particular, the strain and species legends can be used in combination to compare and contrast different sets of data.

For instance, selecting a strain from row 1 and leaving all species selected in row 2, would yield a visualization as shown above. In this figure, the "Not Resistant" strain is selected from the legend. Note that the specimen-level bar charts now gray out specimens without a "Not Resistant" strain. Doing so makes the names of grayed out specimens indistinguishable and lets you focus on what's relevant.

On the other hand, selecting both a strain and a species would yield a visualization as shown below. In this figure, both the "Resistant" strain and the "E_coli" species are selected from the legends. Note that the specimen-level bar charts not only gray out specimens without a "Resistant" strain, but also de-emphasize (by reducing the opacity) specimens without "E_coli" as their species. This combination helps to analyze the data easily, at multiple levels.

![Combined Interactivity of the Dashboard (Strain + Species)](/assets/dashboard-interactivity-2.png)

## Comparative Analysis

Here, I explain how I used the interactive dashboard to answer the questions given in the motivation section, and my observations from doing so.

**Q1 - What are the different species and specimens in the dataset?**

![](/assets/dashboard-q1-1.png)

This question can be answered from the information in specimen-level charts (row 2). The two charts, while representing different attributes on the data (cell count and cell thickness range), show that there are 21 different specimens with varying properties. The colors of the bars represent the species of each specimen, and the legend of colors is displayed on the right end.

**Q2 - What types of blood specimens are missing from the dataset?**

![](/assets/dashboard-q2-1.png)

This question can be answered from the information in species-level charts (row 1). The three charts in row 1. From the first two charts, it is evident that some species do not have specimens from both resistant and non resistant strains. (For instance, B_subtilis has only non-resistant strain specimens, while K_pneumoniae has only resistant-strain specimens). Therefore in the future, specimens from these missing types should be acquired to improve the discriminative and generalizable capabilities of trained ML models.

Also, the stacked height of the specimen count chart indicates that the collected specimens are not balanced across strain. (i.e., the dataset has plenty of resistant-strain specimens, but only a few non resistant-strain specimens).

**Q3 - Can the performance of the machine learning models be explained through the dashboard?**

To facilitate this explanation, I included a confusion matrix from a model trained to classify 7 of the 21 specimens from bacteria cell images.

![](/assets/dashboard-q3-1.png)

All ML models were trained on subsets of specimens. Therefore, while not originally intended, I added a specimen selector to the notebook to minimize blank space and clutter during one-on-one comparison.

![](/assets/dashboard-q3-2.png)

In the trained ML model, only 7 of 21 classes were used, so I first used the specimen selector to filter just those classes. Upon doing this, the dashboard looks as follows.

Using this, I justify why there's less error for the S_aureus_CCUG35600 specimen compared to the others, and the high confusion between K_pneumoniae_A2_23 and E_coli_K12 specimens.

![](/assets/dashboard-q3-3.png)

In this model, S_aureus_CCUG35600 is classified remarkably well compared to other specimens (i.e., very few misclassifications). If we look at that specimen from the interactive dashboard, we see that it has the least range of cell thickness (i.e., a flatter distribution of pixel intensity). This is something that a ML model could easily learn, and therefore can be interpreted as a key contributing factor to its classification performance.

![](/assets/dashboard-q3-4.png)

Similarly, if we consider the K_pneumoniae_A2_23 specimen, we can see that it has the second-highest cell thickness range. However, the model seems to misclassify its cell images mostly as E_coli_K12. If we compare their cell counts from the dashboard, we notice that they're the most dominant specimens in the dataset. Therefore, it is likely that their confusion results from having too many cell images of E_coli_K12 compared to others.

![](/assets/dashboard-q3-5.png)

## Design Decisions

### Row 1

In this row, I concatenate all charts that visualize data at species level. Each chart in this row has a color encoding for the "strain" attribute. Therefore, instead of having multiple legends, I populated just one right-most legend. This also made it possible to directly use the "strain" legend as a point of interaction.

#### Charts 1 and 2
* Mark(s) - Bars
* Channel(s) - Strain (bar color), Species (bar), Cell/Specimen Count (bar height)

Since count is a quantitative attribute, I used it to define the bar height. Since "species" is a nominal attribute, I generated a bar for each species on the x-axis. I encoded "strain" as the bar color. Moreover, instead of having a separate bar for each strain, I decided to stack them over each other. I chose this method to facilitate comparing not only strain-level counts, but also counts regardless of strain.

If a user selects a particular strain from the legend, only counts of that strain are visualized.
#### Chart 3
* Mark(s) - Bars, Ticks
* Channel(s) - Strain (bar color), {Species,Strain} (bar), Cell Thickness μ-σ (bar Y2), Cell Thickness μ+σ (bar Y), Cell Thickness μ (horizontal tick on bar)

Since cell thickness attributes (μ, μ-σ, and μ+σ) are quantitative, I used them to define the vertical extent of each bar. I also added a tick inside each bar to denote the position of "cell thickness μ". I generated a set of column charts using the "species" attribute for faceting. In each facet, I included two bars; one for each strain. To make it easy to distinguish between the bars, I encoded "strain" as the bar color. The reason why I used faceting instead of stacking is to align them on the x-axis. This allows for easy comparison across both species and strain attributes.

Here too, if a user selects a particular strain from the legend, the bars that represent other strains are faded out by reducing their opacity. This brings visual emphasis on the user selection.

### Row 2
In this row, I concatenate all charts that visualize data at specimen level. Each chart in this row has a color encoding for the "species" attribute. Therefore, similar to Row 1, only one right-most legend was populated instead of having multiple legends showing the same data. This also made it possible to directly use the "species" legend as a point of interaction. 
#### Chart 1
* Mark(s) - Bars, Ticks
* Channels(s) - Species (color), Specimen (bar), Cell Count (bar width)

Since "cell_count" is a quantitative attribute, I used it to populate the bar width. Since "specimen" is a nominal attribute, I generated a bar for each species. I encoded "species" as the bar color, and sorted the bars in the descending order of "cell_count". The goal of doing so was to enable users to immediately know which specimen had the most number of bacteria cells (based on ordering and length), and whether or not two specimens are from the same species (based on color). I also offset the y-axis labels to overlay the bars, so that it does not consume unneeded space on the left.

If a user selects a particular "strain" from the legend in row 1, the bars that represent other strains are grayed out. This effectively hides the other bars without introducing too much blank space. Similarly, If a user selects a particular "species" from the legend in row 2, the bars that represent other species are faded out by reducing their opacity. In combination, this adds visual emphasis on the user selection at multiple levels.

#### Chart 2
* Mark(s) - Bars
* Channel(s) - Species (color), Specimen (bar), Cell Thickness μ-σ (bar X2), Cell Thickness μ+σ (bar X), Cell Thickness μ (vertical tick on bar)

Since cell thickness attributes (μ, μ-σ, and μ+σ) are quantitative, I used them to define the horizontal extent of each bar. I also added a tick inside each bar to denote the position of "cell thickness μ". Since "specimen" is a nominal attribute, I generated a bar for each specimen. I encoded "species" as the bar color, and sorted the bars in the descending order of "cell_thickness_μ+σ". The goal of doing so was to enable users to immediately know which specimen had the highest cell thickness (based on bar ordering), and whether or not two specimens are from the same species (based on color). I also offset the y-axis labels to overlay the bars, so that it does not consume unneeded space on the left.

Here too, if a user selects a particular "strain" from the legend in row 1, the bars that represent other strains are grayed out. This effectively hides the other bars without introducing too much blank space. Similarly, If a user selects a particular "species" from the legend in row 2, the bars that represent other species are faded out by reducing their opacity. In combination, this adds visual emphasis on the user selection at multiple levels.

## Development Process

I started the interactive dashboard by attaching the processed data files to Observable. These processed data files consist of two files. One is a JSON file consisting of data such as species, and information related to each specimen (bacteria type, strain, gram, morphology). The other is a CSV file containing statistics of the bacteria including bacteria type, count, mean, maximum, and standard deviation. Then, I merged those two data files based on the bacteria type (label) and created a new table which contains all the information from both data files. I called this "original_data".

Then, I defined the specimen set consisting of unique specimens in the dataset. Then I implemented a specimen selector using Inputs.checkbox() function to select the one or more specimens that the user is interested in. Similarly, I implemented a species selector and a strain selector. Next, I implemented a function to filter the "original_data" based on the selected specimen(s) by the user and store it in a variable called "data_specimen_level". I utilized the "data_specimen_level" to filter data at species level and created "data_species_level". To create "data_species_level", I utilized "data_specimen_level" and grouped by "species", "gram", "morphology", and "strain" and calculated the specimen count, cell count, μ, σ, max, and min by using the .rollup() function.

As the next step, I defined scales and axes for the charts I plan to show in the dashboard. Then, I used vega-lite to create charts which compare statistics of bacteria cells in species level and specimen level for different strains. 

In the first row, I included charts that visualize data at species level. The charts I included in this row are: (1) specimen count for each species, (2) cell count for each species, and (3) cell thickness of each species. I decided to encode a different color to each "strain" (Orange - Resistant , Blue - Not Resistant) for the charts in this row, so that I could provide a single legend for the users to interact with. All three charts in this row are based on strain selector and "data_species_level". For the specimen count and the cell count for each species chart, I experimented with different types of visualizations such as having a separate bar for each strain and stacking bars for each strain over each other. Among those charts, I decided to keep the stack chart where each strain is stacked over each other to facilitate comparing counts regardless of strain. In contrast to the first two charts, I used separate bars; one for each strain for the cell thickness of each species chart. I implemented the charts in a way that, if a user selects a single strain from the legend, the bars that represent the other strain are faded out to bring the visual emphasis on the user selection. Developing the faceted view for the third chart was really time consuming, as I could not find any reference to get my chart with both layers and facets to work. After a while I found a working reference that used the .facet() command and .data() command after defining the .layer(), which I found to work in my case as well.

In the second row, I included charts that visualize data at specimen level. The charts I included in this row are: (1) cell count for each specimen, and (2) cell thickness range for each specimen. In here as well, I decided to provide a single legend for the users to interact with. I encoded a different color to each "species" for the charts in this row. In both charts, I encoded "species" as the bar color, and sorted the bars in the descending order of "cell_count" and "cell_thickness" to enable users to know which specimen had the most number of bacteria cells and the highest cell thickness respectively. Corresponding to the user's selection of the "strain" from the legend in the first row, the bars that represent other strains are grayed out. If a user selects a "species" from the legend in the second row, the bars that represent other species are faded out.

Overall, the dashboard lets users explore statistics of bacteria cells by choosing different strains and species from the legends on the right. I added interactivity using tooltips and legends so that users can focus on different strains/species.

In the dashboard, I wanted to initially use image data and implement ULCA (from my paper presentation) to identify important features. But for the interest of time, this was not possible as I didn't find a JS implementation of ULCA that I could use. In addition, I also wanted to display charts indicating morphology and gram attributes of the dataset, which I couldn't implement and add to the dashboard.

### Time Breakdown
1. Generating a CSV with statistics of bacteria cell images: ~2 hours
2. Combining and manipulating data: ~2 hours
3. Creating charts in vega-lite, adding interactivity: ~ 24 hours (including reading blogs/notebooks to fix bugs)
4. Creating dashboard, concatenating charts, refining look and feel: ~ 12 hours
5. Writing the report: ~ 10 hours

**Total time spent: ~ 50 hours**

Since finding good documentation/examples on how to perform certain visualizations/interactions from the vega-lite API was really, really hard, it took more time than I expected to fix bugs in the charts. I also spent a considerable amount of time refining the dashboard (things like improving positioning, coloring, and interactivity) to make the most out of the dashboard. By the end, I was able to create the dashboard in a manner that let me explore different statistics of bacteria cells than I could have otherwise. Upon implementation, it allowed me to select different strains/species from the vega-lite legends, and optionally, to limit the analysis to certain specimens through observable checkboxes. I also spent a reasonable amount of time testing out different ways to place the charts, legends, etc, and how to color them, so that I could make the best of it.

## Concluding Remarks

In my opinion, the most useful aspect of this dashboard is being able to focus on different strains/species/specimens, and those visualizations to quantify why the trained ML models behave the way they do. The knowledge that I gained through this course was really helpful and definitely improved my way of thinking about visualizations. I hope to use this knowledge to create increasingly good visualizations of information in the future.


## References 

* [Arquero API Reference](https://uwdata.github.io/arquero/api/)
* [Vega-Lite API Reference](https://vega.github.io/vega-lite-api/api/)
* [Bar Chart Walk-through / NYU Visualization / Observable](https://observablehq.com/@nyuvis/bar-chart-walk-through)
* [Vega-Lite-API Layer + Facet / John Alexis Guerra Gómez / Observable](https://observablehq.com/@john-guerra/vega-lite-api-layer-facet)
* [Header | Vega-Lite](https://vega.github.io/vega-lite/docs/header.html)

## Appendix - Domain-Specific Facts

1. Technically, QPM measures are proportional to both thickness and refractive index of the specimen. However, it’s assumed that bacteria cells have a constant refractive index, thereby allowing us to interpret QPM measures as proportional to specimen thickness only.
2. While I refer to each specimen/patch as an "image", their point values indicate cell thickness, not light intensity.
3. QPM provides a relative phase measure at each point of the blood specimen. Since phase ranges from [0, 2π], measures exceeding 2π create discontinuities, as they are wrapped down to 0. Therefore, phase is first "unwrapped" (i.e., adding multiples of 2π at discontinuities) to create a continuous signal. Therefore, an incorrect phase unwrapping would introduce noise.

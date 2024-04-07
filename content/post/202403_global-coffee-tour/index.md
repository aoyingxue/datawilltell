---
title: Global Coffee Tour
description: The global coffee tour includes a multi-continental overview from the production of coffee beans to the infamous local coffee shops.
slug: global-coffee-tour
date: 2024-04-04
image: cover.png
categories:
    - Personal Projects
tags:
    - Tableau
    - Data Visualization
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
## Dashboard

It's better to interact with it at Tableau Public ([Link]()). I'd be much appreciate it if you like it and add it as your favorite.

## Brainstorming ideas

In March, 2024, I made it one of my new year resolutions to make at least 10 complete personal projects. I was a data operations specialist, or technically a business analyst. The daily work and ad hoc work involved a lot of data cleaning, since the company I was in was on the progress of building a ERP system but it's still in the earliest stages. While I was wondering in a coffee shop about what other interesting topics I can dive in outside work, having the accessible data and playing around the data without social consequences, I said to myself, why not coffee since I'm a coffee lover?

Since then I browsed through websites and books, searched for data about coffee, ranging from coffee bean production, coffee types, coffee shops, and I came across a book named "*Lonely Planet's Global Coffee Tour*". It's published in 2018, rated high on Amazon, and available in the local library. I borrowed it, read it, and collected information and self-made a table including unique coffee styles in 36 countries across 7 continents, provided services and ratings of 164 coffee shops around the world. ([Google sheet link is here](https://docs.google.com/spreadsheets/d/1L81u0-yGyPNaKl_bZnY8OKhvM4T0b7e2Z52gGJGsGx4/edit?usp=sharing).)

Using this data as the main source, I was hoping to set up a simple coffee info & recommendation system.

## The way I did it and challenges along the way

### Tableau map limitations when drawing a single continent

What I had in mind is to show one continent at each dashboard, but Tableau doesn't have continent attribute when it comes to draw maps. Good thing is ArcGIS provides shape files for each continent.

Next thing is to standardize the aliases. For example, in ArcGIS data, North America and South America, as the book mentioned, are both recorded as Americas. Before I put the data in Tableau, firstly I had to align them, so that the same thing has the same name in different data sources.

![Coffee shop map of Americas](image-20240331231149117.png)

### Paginate the table in Tableau

I collected the information of as many as 164 coffee shops from the book, which cannot be shown in a table at the same time without scrolling your mouse. So what I had in mind is a next/previous button to replace the page scroller, something like this:

![image-20240404173749465](image-20240404173749465.png)

1. Create 2 parameters: Page Size and Current Page as below. You could choose a fixed value for page size, or you could make it a parameter to easily change it to the size until it's fit for the design.

   ![Current Page](image-20240331231818577.png)![](image-20240401091531503.png)
2. Using the parameters above to create calculations that get the real page number for each row based on page size you set up.

   ![](image-20240401092527458.png)

   If you put it in rows as a discrete variable, you would see every 10 (aka page size) rows are assigned to the same value of page number, which can be used as a filter.

   ![](image-20240401092639728.png)

   So to achieve this, create another calculation as a filter, simply by calculating whether the parameter Current Page is equal to the table calculation Page Number and returning a boolean.

   ![](image-20240401092920048.png)

   If you now drag the Page Filter to Filters and select only True, you could see it's exactly showing the number of rows as the Page Size.

   ![Page Filter True](image-20240401093201531.png)![](image-20240401093236853.png)

3. Build the buttons

   1. Firstly, you have to know which page number is the last page so that you won't end up staring at a blank page.

      ![Last Page](image-20240401093412741.png)
   2. Next, the pagination label using a number placeholder.

      What I want to achieve is a series of buttons that can do "return to the first page", "to the previous page", an indicator showing the current page number, "to the next page" and "to the last page".

      The first thing is to find a number placeholder that won't be changed over time or filters. I made up a simple excel sheet and connect it with coffee store data.

      ![](image-20240401143156689.png)

      Using it we can create a calculation to give those 5 buttons a shape/number label. Sort the field ascending by Number (1,2,3,4,5).

      ![](image-20240401143057909.png)![](image-20240401144117550.png)

   3. The buttons are correctly shown but each button should be assigned to a real value which can be used to change the current page number parameter.
   
      ![Page Value](image-20240401150638763.png)
   4. Set up an action to change parameter.
   
      ![Parameter Action](image-20240401150602264.png)
   5. Create a calculation based on which we format the colors.
   
      ![Button Color](image-20240401155422224.png)![](image-20240401155512823.png)
   

Everything is perfect, just as I imagined.

![](image-20240404173921287.png)

However, when I wanna interact with the map and let map control stores in which country show in the table, there's a problem. Because I had to use a LOD when calculating the last page number of the table, it won't work when any table filters applied. So I made a little workaround to filter country when clicking the country on the map. I added a parameter \[country\] in the calculations when calculating the page filter and last page number as below. I'll do the similar tricks with other continents later. 

![](image-20240405211431452.png)

### Layout design 

1. Text containers:

   1. I'd like 4 containers to showcase each country's coffee styles, coffee pairings, coffee languages, and preferences in words. What I had in mind is frames with rounded edges but containers in Tableau cannot do that. So I turned to Figma to draw it and export it as a png image.

      ![Figma](image-20240406195303672.png)

   2. After adding it as a floating background image on Tableau, I put the text containers and text dashboards in it and then put a blank container floating above all so that users won't accidentally click on these. 

      ![Tableau](image-20240406195505491.png)

2. Arrow

---
date: 2024-12-19 22:42
title: MRI Imaging - Cleaning for SNR improvement
---
Use Nilearn's `clean_img` function 

Low-pass filtering improves specificity
High-pass filtering should be small to keep sensitivity
- [?] What does low-pass and high-pass filtering do in this context?

1. Get TR value of the fMRI image
2. Detrend the image
3. Plot the original and detrended timecourse of a random voxel

- [?] What is detrending?
>[!note] Detrending
>Detrending is the removal of trends from a time series data. By removing trends, we can uncover important patterns in the data that were occluded by the trends
>>[!definition] Trend
>>A trend is the change in mean of a time series data over time

https://stackoverflow.com/questions/75142372/adding-and-removing-trend-to-time-series-data <- The plot here made me understand detrending easily

A **voxel** is a 3D cuboid from an fMRI image. Important to improve SNR because I should find which voxel --> which activation in the brain
Low-frequency drifts can happen in the data due to scanner instabilities, head motion changes, physiological fluctuations like breathing etc. 


- [?] What is a time course of a voxel? 

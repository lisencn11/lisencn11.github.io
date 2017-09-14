---
layout: post
title: Reading Code Complete - Working Classes 0
date: 2017-09-13
categories: blog
tags: [study]

---

# Guidelines for ADT

### Build or use typical low-level data types as ADTs, not as low-level data types

For example, build a Tasks class in CorvoService instead of using List\<Task>.

### Treat common objects such as files as ADTs

For example, in Corvo completeTask API, I write download file as a routine. But now I think I should write a class called Downloader which has a **download(Path path, FileLink fileLink)** method.

### Treat even simple items as ADTs

### Refer to an ADT independently of the medium it’s stored onRefer to an ADT independently of the medium it’s stored on

For example, in Corvo I write a class called S3Utils to get report, but what if one day we no longer use S3 for storage? Try to make the **names of classes** and **access routines** independent of howthe data is stored.

---
title: "MySQL database and OpenRefine workflow"
meta_title: "MySQL database and OpenRefine workflow - Graham H. Jensen"
date: "2024-06-22"
image: "/images/cmmp_database-sketch.png"
categories: ["Archive or Repository", "Dataset", "Workflow"]
author: "Graham H. Jensen"
tags: ["Canadian literature", "Data cleaning", "Data transformation", "English literature", "Experimental", "Modernism", "MySQL", "OpenRefine", "Periodicals", "PHP"]
draft: false
---
**Description**: In June 2024, I took Harvey Quamen and Jon Bath's "Databases for Humanists" course at the [Digital Humanities Summer Institute](https://dhsi.org/). During the week, we covered topics such as database design, MySQL, basic to advanced SQL queries, and (briefly) how to connect to databases with Python or using other methods. While I had some familiarity with these topics already, the course provided me with the opportunity to design and build my own MySQL database from the ground up, which I did using [my open access dataset](https://www.modernistmags.ca/research/data/) from the [*Canadian Modernist Magazines Project*](https://www.modernistmags.ca/). (You can read about the CMMP in [one of my other posts](https://ghjensen.com/dh/canadian-modernist-magazines-project/)).

On the final day, I even had the opportunity to demo one of my discoveries: how OpenRefine, a tool I've been using for some of my other work at the University of Victoria, can be used to connect to MySQL database tables, perform advanced data transformations with relative ease, or (my favourite part) take advantage of OpenRefine's powerful but seemingly little-known "templating" feature to export my tabular dataset in the form of customized SQL `INSERT` statements. For example:

```sql
    /* Prefix */
INSERT INTO text (id,title,contribution_id,issue_id,pagestart,pageend) VALUES 

/* Row template*/
{{"(" + if(cells["text_id"].value==null,"",cells["text_id"].value) + if(cells["SQLsafetitle"].value==null,"",",'" + cells["SQLsafetitle"].value + "'") + if(cells["text_id"].value==null,"","," + cells["text_id"].value) + if(cells["issue_id"].value==null,"","," + cells["issue_id"].value) + if(cells["Page Range: Start"].value==null,"",",'" + cells["Page Range: Start"].value + "'") + if(cells["Page Range: End"].value==null,"",",'" + cells["Page Range: End"].value + "'") + ")"}}

/* Separator */
,
```

Working from a single CSV file, I was able to use similar statements for each table to create an SQL file. I then pasted the resulting text from these files into the MySQL Command Line Client, one at a time, to create and populate each of my 6 MySQL tables. The largest of these tables—corresponding to unique texts digitized and published as part of the *Canadian Modernist Magazines Project*, and pictured below—contained 720 rows. So I was very grateful to use this method as an alternative to manual data entry.

**Deliverables**: Using the model for my database's design (above) as a guide, plus the templates I used in OpenRefine, I produced a complete MySQL database of the CMMP's dataset. I can now query this dataset in complex ways. In the next phase of the *Canadian Modernist Magazines Project*, I'd like to add new data to the project's database, including (for example) gender and age info from VIAF records, to do further and more in-depth analyses of authorship.

![screenshot #2](/images/cmmp_finished_text-table.png)

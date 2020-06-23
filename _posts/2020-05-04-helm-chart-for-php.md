---
layout: post
title:  "Helm chart for php application"
date:   2020-05-04 15:40:14 +0100
categories: helm, php
---
# Package helm chart
Create an archive which contains the helm chart:
    
    helm package php

Generate the index.yaml

     helm repo index . --url https://nicolas2lee.github.io/<repo-url>
     
# Use github as helm chart repo
For github users, we can use github repo as a helm chart repo. Alternatively you can choose cloud storage like s3.

    settings -> github pages
   
![github page](/assets/images/2020-05-04-helm-chart-for-php-github-page.png)
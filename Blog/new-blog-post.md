---
title: "New Blog Post"
description: "Enter your post description here"
author: "Your Name"
date: "2025-04-10"
featured_image: "https://camo.githubusercontent.com/678920e8f8c62e433e03c096612659d81ae96d9befda00d54861379aba81ed5f/68747470733a2f2f63646e2e626f6c6462692e636f6d2f77702f70616765732f64617368626f617264732f68722f68722d656d706c6f7965652d64657461696c732e706e67"
categories: ["Uncategorized"]
tags: []
is_featured: false
---

## Hello World

Start writing your blog post content here...

```
private BlogPost BlogPost { get; set; } = new();
private bool IsLoading { get; set; } = true;
private bool IsSaving { get; set; }
private bool IsMetadataPanelVisible { get; set; } = true;
private bool IsEditFormVisible { get; set; } = true;
private string? ErrorMessage { get; set; }
private string? SaveSuccessMessage { get; set; }
private bool IsNewPost => string.IsNullOrEmpty(FilePath);

private Confirmation? ConfirmationDialog { get; set; }

protected override async Task OnInitializedAsync()
{
    if (IsNewPost)
    {
        // Initialize new blog post
        BlogPost = new BlogPost
            {
                Metadata = new FrontMatter
                {
                    Title = "New Blog Post",
                    Description = "Enter your post description here",
                    Author = "Your Name", // Could be loaded from user settings
                    Date = DateTime.Now.ToString("yyyy-MM-dd"),
                    Categories = new List<string> { "Uncategorized" },
                    Tags = new List<string>()
                },
                Content = "## Hello World\n\nStart writing your blog post content here..."
            };

        IsLoading = false;
    }
    else
    {
        await LoadPost();
    }
}
```

![alt text](https://example.com/image.jpg)https://example.com)

- Item 1
- Item 2
- Item 3

![alt text](https://cdn.boldbi.com/wp/pages/dashboards/hr/hr-employee-details.png)


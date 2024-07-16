---
layout: post
title: WordPress Development Best Security Practices
date: 2024-07-16 00:00:00 -0500
description:
categories:
author: Angel Baez
---

Today's post will cover WordPress security.

Most of our clients at my current company use WordPress. While it's a great platform for building websites, it's also a popular target for hackers. Here are some best practices I follow to keep WordPress sites secure:

1. Update WordPress, themes, and plugins regularly, please.
2. Use strong passwords, please.
3. Avoid using a popular pagebuilder like Elementor, Divi, or WPBakery. I've seen sites get hacked because of these plugins. I think the built-in editor is good enough. Of course unless you're building a more complex site.
4. Any output should be escaped. `esc_url()` all the things.
5. When there's a WordPress function, use it. Don't reinvent the wheel.
6. Always use `$wpdb` functions
7. Use `$wpdb->prepare()` to escape SQL queries.
8. Use nonces to protect forms.

## Additional Resources

- [WordPress Security](https://wordpress.org/support/article/hardening-wordpress/) Your bible if you're taking WordPress security seriously.
- [Theme Handbook Security](https://developer.wordpress.org/themes/advanced-topics/security/) If you're a theme developer, this is a must-read, (which my post covers all of it).

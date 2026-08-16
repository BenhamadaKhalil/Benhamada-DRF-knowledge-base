---
title: Map of Content
type: moc
tags: [noetix, moc, index]
updated: 2026-08-16
---

# 🗂️ Map of Content

> Every note in Noetix, in reading order. This is the index — [[Learning Path]] is the guide.

---

## 01 · App Design

- [[APP Design]] — the brief: 19 endpoints, auth, browsable API
- [[Technologies]] — Django, DRF, Postgres, Docker, GitHub Actions and why each
- [[Endpoint API.canvas|🔌 Endpoint API (canvas)]]

## 02 · System Setup

- [[1.Required Tools]]
- [[2.Verify Installation]]

## 03 · Test Driven Development *(concept)*

- [[Test Driven Development]] — the red/green/refactor loop
- [[TDD]] — the short version
- [[Test Web Request]]

## 04 · Project Setup

- [[1.overview]]
- [[2.Create GitHub Repo]]
- [[3.Docker with Django]]
- [[4.Python Requirements]]
- [[5.Dockerfile]]
- [[6.docker-compose.yml]]
- [[7.Linting&Testing]]
- [[8.Configure Flake8]]
- [[9.Create the Django Project]]
- [[10.Run the Django Development Server]]
- [[11.Section Summary]]

## 05 · GitHub Actions

- [[GitHub Actions]]
- [[1.Automated Testing & Linting]]
- [[2.config GitHub Actions]]
- [[3.Create the Workflow Config]]
- [[4.Test the Workflow]]

## 06 · Configure Database

- [[1.Database Configuration with PostgreSQL & Docker Compose]]
- [[2.Adding PostgreSQL Service]]
- [[3.Configure Django to Use PostgreSQL]]
- [[4.Installing psycopg2]]
- [[5.Configure settings.py for PostgreSQL]]
- [[6.Fixing Database Race Condition]]
- [[7.Creating the Core App for Custom Commands]]
- [[8.Create wait_for_db]]
- [[9.Implement `wait_for_db`]]
- [[10.Database Migrations]]
- [[11.Run `wait_for_db`]]

## 07 · TDD with Django

- [[1.Creating Your First Django Test]]
- [[2.Test Driven Development]]
- [[3.What Is Mocking]]
- [[4.Common Django Test Issues]]
- [[5.Testing Web Requests]]

## 08 · Create User Model

- [[1.Custom User Model in Django]]
- [[2.Custom User Model – Design]]
- [[3.Writing Unit Tests]]
- [[4.Implementing a Custom User Model]]
- [[4.Implementing a Custom User Model 1]]
- [[5.normalize email in `create_user`]]
- [[6.Add “email is required” feature]]
- [[7.Add “create superuser”]]
- [[8.testing the Custom User Model]]

## 09 · Setup Django Admin

- [[1.Overview & Customization]]
- [[2.Write Unit Tests for Admin]]
- [[3.Enable Admin for Custom User Model]]
- [[4.Fix the User Change Page]]
- [[5.Support Creating Users]]

## 10 · Documentation API

- [[1. Why It Matters & How We’ll Do It]]
- [[2.Automated API Documentation in Django REST Framework]]
- [[3.installing & Enabling DRF Spectacular]]
- [[4.Enabling Swagger Documentation URLs]]
- [[5.Testing Swagger Documentation]]
- [[6.Section Summary]]

## 11 · Build User API

- [[User API design]]
- [[1.Design & Endpoints]]
- [[2.Creating the `user` App]]
- [[3.User API Tests]]
- [[4.Authentication in Django REST Framework]] ⭐
- [[5.Implementing Create User API]]
- [[6.Token API Tests]]
- [[7.Custom Token Authentication]]
- [[8.DRF – Tests for “Manage User” API]]
- [[9.Implement “Manage User”]]
- [[10.Test the User API in the Browser]]
- [[11.Section Summary]]

## 12 · Build Recipe API

- [[Recipe API – Overview]]
- [[1.Recipe API Endpoints]]
- [[2.APIView vs ViewSet]] ⭐
- [[3.Creating the Recipe Model Test]]
- [[4.Implementing the Recipe Model in Django]]
- [[5.Creating the `recipe` App for API Endpoints]]
- [[6.Testing the Recipe List API Endpoint]]
- [[7.create an API endpoint to list recipes]]
- [[8.Write tests for recipe detail API Follow Along English]]
- [[9.Recipe Detail Serializer]]
- [[10.Testing Recipe Creation via API]]
- [[11.Implementing Recipe Creation in DRF]]

## 13 · Build Tag API

- [[Tags Section — Detailed Summary]]
- [[1.Tag API — Design and Endpoints]]
- [[2.Writing the Tag Model and Listing Tags]]
- [[3.Write tests for creating tags Follow Along]]
- [[4.Implementing Tag Creation]]
- [[5.test for Updating Recipe Tags via API]]
- [[6.Implement update recipe tags feature Follow Along]]
- [[Creating Tags]]
- [[Nested Serialization]] ⭐
- [[Tuple Unpacking]]

## 14 · Build Ingredient API

- [[Ingredient API]]
- [[1.Ingredient API — Design and Full Walkthrough]]
- [[Create Ingredients Feature — Implementation Notes]]
- [[Refactoring]] ⭐

## 15 · Build Image API

- [[🖼️ Adding Images to Recipe Model (Django)]]
- [[🗂️ Django Static & Media Files (with Docker)]]
- [[📤 Recipe Image Upload API — Tests (Django REST)]]
- [[📸 Recipe Image Upload API — Implementation]]
- [[path generator function]]
- [[upload image function]]
- [[test uploading image]]

## 16 · Implement Filtering

- [[Filtering & API Improvements]]
- [[🧩 Filtering Recipes by Tags & Ingredients (with API Documentation)]]
- [[🧱 Filtering Tags & Ingredients]]

## 17 · Deployment

- [[1.From Dev Compose to Production]]
- [[2.uWSGI and the Production Dockerfile]]
- [[3.nginx as a Reverse Proxy]]
- [[4.Environment Variables and Secrets]]
- [[5.Deploying to a Server]]
- [[6.Post-Deploy Checklist]]

## 18 · All Fixes

- [[1.Django Refresher - Projects, Apps and the Five Files]]
- [[2.Errors You Will Actually Hit]]

## 19 · Production DRF *(beyond the course)*

- [[0.Production Readiness Checklist]] ⭐
- [[1.Pagination]]
- [[2.Throttling and Rate Limiting]]
- [[3.Caching]]
- [[4.The N+1 Problem]] ⭐
- [[5.Custom Permissions]]
- [[6.Token vs JWT vs Session]]
- [[7.Signals]]
- [[8.Transactions]]
- [[9.Background Jobs with Celery]]
- [[10.Security Checklist]] ⭐
- [[11.Logging and Observability]]
- [[12.Versioning an API]]

## 20 · Reference *(cheat sheets)*

- [[0.Reference Index]]
- [[1.HTTP Status Codes]]
- [[2.Serializer Field Matrix]]
- [[3.ViewSet and Router Cheatsheet]]
- [[4.Django ORM Cookbook]]
- [[5.Testing Patterns]]
- [[6.Docker and Compose Commands]]
- [[7.Model Field Reference]]
- [[8.Glossary]]

## 21 · Interview & Real World

- [[0.Interview Index]]
- [[1.DRF Interview Questions]]
- [[2.Common Bugs and Fixes]]
- [[3.Architecture Decision Records]]
- [[Debugging Decision Tree]] ⭐

## 22 · Django Core *(the framework underneath DRF)*

- [[0.Django Core Index]] ⭐
- [[1.What's New in Django 6.1]] ⭐
- [[2.Settings and Configuration]]
- [[3.Middleware]]
- [[4.QuerySets in Depth]]
- [[5.Model Validation and Constraints]]
- [[6.Forms and Validation]]
- [[7.The Auth System]]
- [[8.The Cache Framework]]
- [[9.Security Features]]
- [[10.Async Django]]
- [[11.Email and Mailers]]
- [[12.Management Commands]]
- [[13.Internationalization]]

## 23 · What to master next

- [[DRF Mastery Roadmap]]

---

## 🧩 Concept layer

Short definitions for the ideas that recur across sections. Most declare **aliases**, so `[[TokenAuthentication]]`, `[[Token Authentication]]` and `[[Authentication in DRF]]` all land on one page.

- [[0.Concept Index]] — all of them, grouped

**Framework** — [[Django REST Framework]] · [[REST API Design]] · [[Django Project Structure]]
**Views** — [[APIView]] · [[Generic Views]] · [[ViewSet]] · [[Routers]] · [[perform_create]] · [[reverse URL resolution]]
**Serializers** — [[Serializers]] · [[Serializer Validation]] · [[Serializer create and update]] · [[Serializer Context]]
**Auth** — [[Token Authentication]] · [[Authorization Header]] · [[ObtainAuthToken]] · [[JWT]] · [[SessionAuthentication]] · [[Permissions]] · [[request.user]] · [[Password Hashing]] · [[Custom User Model]]
**Data** — [[Django Models]] · [[ForeignKey Relationships]] · [[ManyToMany Relationships]] · [[get_or_create]] · [[Django Migrations]] · [[Decimal vs Float]]
**Testing** — [[APIClient]] · [[force_authenticate]] · [[Python getattr]]
**Interfaces** — [[Django Admin]] · [[Browsable API]] · [[OpenAPI]] · [[PATCH vs PUT]]

---

## Noetix system files

- [[Noetix Home]]
- [[Learning Path]]
- [[Style Guide]]
- [[Note Template]]
- [[Changelog]]

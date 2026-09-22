eShop Modernized Web Forms — Product Overview
Purpose
This application is a demo product catalog management system based on the Microsoft eShopModernizing sample. It lets staff browse, create, edit, and delete catalog products, and is primarily used as a modernization exercise sandbox — a realistic legacy ASP.NET Web Forms app (.NET Framework 4.7.2, EF6, SQL Server) that teams use to practice modernization tooling (e.g. Copilot app modernization, containerization, migration to modern .NET).

It is not a full storefront — there's no shopping cart, checkout, or customer-facing ordering flow. It focuses purely on the catalog domain: products, brands, types, pricing, stock, and images.

Domain Language (Glossary)
Catalog Item — a single product record: Name, Description, Price, Picture, Brand, Type, and stock fields.
Catalog Brand — the manufacturer/brand of a product (e.g. ".NET", "Azure"). Simple lookup: Id + Brand name.
Catalog Type — the product category (e.g. "Mug", "T-Shirt"). Simple lookup: Id + Type name.
Available Stock — current quantity in inventory for a Catalog Item.
Restock Threshold — the stock level at which a reorder should be triggered.
Max Stock Threshold — the maximum quantity that can be held in stock (logistics constraint).
On Reorder — flag indicating a Catalog Item is currently being reordered.
Picture File Name / Picture Uri — the product's image, served either from the local Pics folder or Azure Blob Storage, with dummy.png as the default placeholder.
Paginated Items — the pattern used to return a page of Catalog Items at a time (pageSize/pageIndex) rather than the full catalog, for performance on large data sets.
Mock Data vs. Live Data — the app can run against in-memory mock services (UseMockData=true in Web.config) or against a real SQL Server database via Entity Framework 6, controlled entirely by configuration — no code changes needed to switch.
Key Building Blocks (from code)
Models: CatalogItem, CatalogBrand, CatalogType, plus CatalogDBContext (EF6) and seeding logic.
Services: ICatalogService (real EF6-backed CatalogService) exposes GetCatalogItemsPaginated, FindCatalogItem, GetCatalogBrands, GetCatalogTypes, CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem. A mock implementation exists for running without a database.
Catalog pages: ASP.NET Web Forms pages for Browse/List, Create, Edit, Details, and Delete, styled with Bootstrap.
Configuration-driven behavior: Web.config flags control mock vs. real data, Azure Storage vs. local Pics folder, Azure AD auth, and Key Vault-backed secrets.
Intended Outcomes
Give catalog managers a simple, reliable way to view, filter, and manage the product catalog (brand/type filtering, pagination for large catalogs, correct image rendering with fallbacks).
Preserve a realistic, EF6/Web Forms legacy footprint so it remains a faithful target for modernization exercises (this is why scope on backlog items like eShop-web-forms-modernization#1 explicitly excludes migrating off Web Forms or changing the data model — those are separate, deliberate modernization phases).
Support flexible deployment/configuration (mock data for quick demos, SQL Server for realistic runs, optional Azure AD auth and Blob Storage) without code changes.
Serve as the baseline against which future modernization work (UI rebuilds, framework upgrades, containerization) is measured.

<template>
  <main class="page-shell">
    <ais-instant-search index-name="demo_ecommerce" :search-client="searchClient">
      <ais-configure :hits-per-page.camel="12" />

      <header class="header">
        <div>
          <p class="eyebrow">Vue InstantSearch</p>
          <h1>Demo Store</h1>
        </div>

        <ais-powered-by />
      </header>

      <section class="search-panel">
        <ais-search-box placeholder="Search products..." />
      </section>

      <div class="layout">
        <aside class="filters">
          <section class="filter-card">
            <h2>Brand</h2>
            <ais-refinement-list attribute="brand" searchable />
          </section>

          <section class="filter-card">
            <h2>Categories</h2>
            <ais-hierarchical-menu
              :attributes="[
                'hierarchicalCategories.lvl0',
                'hierarchicalCategories.lvl1',
                'hierarchicalCategories.lvl2'
              ]"
            />
          </section>

          <section class="filter-card">
            <h2>Price</h2>
            <ais-range-input attribute="price" />
          </section>

          <section class="filter-card">
            <h2>Rating</h2>
            <ais-rating-menu attribute="rating" />
          </section>

          <ais-clear-refinements />
        </aside>

        <section class="results-area">
          <div class="toolbar">
            <ais-stats />
            <ais-hits-per-page :items="hitsPerPageItems" />
          </div>

          <ais-state-results>
            <template v-slot="{ results: searchResults }">
              <p v-if="searchResults && searchResults.nbHits === 0" class="empty-state">
                No matching products found.
              </p>
            </template>
          </ais-state-results>

          <ais-hits>
            <template v-slot:item="{ item }">
              <article class="product-card">
                <img
                  v-if="item.image"
                  class="product-image"
                  :src="item.image"
                  :alt="item.name || 'Product image'"
                />

                <div class="product-content">
                  <h2>
                    <ais-highlight attribute="name" :hit="item" />
                  </h2>

                  <p v-if="item.brand" class="brand">{{ item.brand }}</p>
                  <p v-if="item.categories" class="category">{{ formatCategories(item.categories) }}</p>

                  <p v-if="item.price" class="price">${{ item.price }}</p>
                </div>
              </article>
            </template>
          </ais-hits>

          <ais-pagination @page-change="scrollToTop" />
        </section>
      </div>
    </ais-instant-search>
  </main>
</template>

<script>
import { liteClient as algoliasearch } from 'algoliasearch/lite';
import 'instantsearch.css/themes/algolia-min.css';

export default {
  name: 'App',

  data() {
    return {
      searchClient: algoliasearch(
        'B1G2GM9NG0',
        'aadef574be1f9252bb48d4ea09b5cfe5'
      ),
      hitsPerPageItems: [
        { label: '12 per page', value: 12, default: true },
        { label: '24 per page', value: 24 },
        { label: '48 per page', value: 48 },
      ],
    };
  },

  methods: {
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },

    formatCategories(categories) {
      if (Array.isArray(categories)) {
        return categories.join(' › ');
      }

      return categories;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f6f7fb;
  color: #1f2937;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

.page-shell {
  width: min(1180px, calc(100% - 32px));
  margin: 0 auto;
  padding: 32px 0 48px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  margin-bottom: 24px;
}

.header h1 {
  margin: 0;
  font-size: 38px;
  line-height: 1.1;
}

.eyebrow {
  margin: 0 0 6px;
  color: #6b7280;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.search-panel {
  margin-bottom: 24px;
}

.layout {
  display: grid;
  grid-template-columns: 280px minmax(0, 1fr);
  gap: 24px;
}

.filters,
.results-area,
.search-panel {
  min-width: 0;
}

.filter-card,
.results-area,
.search-panel {
  border: 1px solid #e5e7eb;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 12px 32px rgba(15, 23, 42, 0.06);
}

.search-panel {
  padding: 18px;
}

.filters {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.filter-card {
  padding: 18px;
}

.filter-card h2 {
  margin: 0 0 12px;
  font-size: 16px;
}

.results-area {
  padding: 18px;
}

.toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  margin-bottom: 18px;
}

.ais-Hits-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
  margin: 0;
  padding: 0;
}

.ais-Hits-item {
  padding: 0;
  border: 0;
  box-shadow: none;
}

.product-card {
  height: 100%;
  overflow: hidden;
  border: 1px solid #e5e7eb;
  border-radius: 14px;
  background: #fff;
}

.product-image {
  width: 100%;
  height: 180px;
  object-fit: contain;
  padding: 18px;
  background: #f9fafb;
}

.product-content {
  padding: 16px;
}

.product-content h2 {
  min-height: 44px;
  margin: 0 0 8px;
  font-size: 16px;
  line-height: 1.35;
}

.brand,
.category {
  margin: 0 0 6px;
  color: #6b7280;
  font-size: 13px;
}

.price {
  margin: 12px 0 0;
  color: #111827;
  font-size: 22px;
  font-weight: 800;
}

.empty-state {
  margin: 0 0 18px;
  border-radius: 12px;
  background: #fff7ed;
  padding: 14px 16px;
  color: #9a3412;
}

.ais-Pagination {
  margin-top: 24px;
}

@media (max-width: 860px) {
  .layout {
    grid-template-columns: 1fr;
  }

  .header {
    align-items: flex-start;
    flex-direction: column;
  }
}
</style>

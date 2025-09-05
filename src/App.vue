<template>
  <div class="container">
    <!-- Category Menu -->
    <div class="category-menu">
      <button 
        v-for="category in categories" 
        :key="category.id"
        :class="['category-btn', { active: selectedCategory === category.id }]"
        @click="selectCategory(category.id)"
      >
        {{ category.name }}
      </button>
    </div>

    <!-- Budget Section -->
    <div class="budget-section">
      <div class="budget-input-wrapper">
        <span class="budget-label">Budget:</span>
        <input 
          :value="budgetFormatted" 
          type="text" 
          class="budget-input-combined"
          @input="handleBudgetInput"
        >
      </div>
    </div>

    <!-- Main Content -->
    <div class="main-content">
      <!-- Item Details (Left) -->
      <div class="item-details">
        <h3>Selected Item</h3>
        <div class="selected-item" v-if="selectedItem">
          <div class="item-name">{{ selectedItem.name }}</div>
          <div class="item-price">₱{{ selectedItem.price.toLocaleString() }}</div>
        </div>
        <div v-else class="selected-item">
          <div class="item-name">No item selected</div>
          <div class="item-price">₱0</div>
        </div>
      </div>

      <!-- Item List (Right) -->
      <div class="item-list">
        <h3>{{ getCurrentCategoryName() }} Items</h3>
        <div class="wishlist-items" ref="itemsList">
          <div 
            v-for="(item, index) in getCurrentCategoryItems()" 
            :key="item.id"
            :class="['wishlist-item', { selected: selectedItem && selectedItem.id === item.id }]"
            @click="selectItem(item)"
            :data-item-id="item.id"
          >
            <div 
              class="item-fill" 
              :style="{ width: item.fillPercentage + '%' }"
            ></div>
            <div class="item-content">
              <span class="item-title">{{ item.name }}</span>
              <span class="item-cost">₱{{ item.price.toLocaleString() }}</span>
            </div>
          </div>
        </div>
        <button class="add-item" @click="toggleAddForm">
          + Add Item
        </button>
        
        <!-- Add Item Form -->
        <div v-if="showAddForm" class="add-item-form">
          <input 
            v-model="newItemName" 
            type="text" 
            placeholder="Item name"
            class="form-input"
            @keyup.enter="addNewItem"
          >
          <input 
            v-model.number="newItemPrice" 
            type="number" 
            placeholder="Price (PHP)"
            class="form-input"
            @keyup.enter="addNewItem"
          >
          <div class="form-buttons">
            <button @click="addNewItem" class="btn-add">Add</button>
            <button @click="cancelAddForm" class="btn-cancel">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Sortable from 'sortablejs'

export default {
  name: 'WishlistApp',
  data() {
    return {
      budget: 10000,
      selectedCategory: 'groceries',
      selectedItem: null,
      showAddForm: false,
      newItemName: '',
      newItemPrice: 0,
      sortableInstance: null,
      categories: [
        { id: 'groceries', name: 'Groceries' },
        { id: 'business', name: 'Business' },
        { id: 'clothes', name: 'Clothes' },
        { id: 'software', name: 'Software' }
      ],
      items: {
        groceries: [
          { id: 1, name: 'Rice 5kg', price: 1000, fillPercentage: 0 },
          { id: 2, name: 'Cooking Oil', price: 5000, fillPercentage: 0 },
          { id: 3, name: 'Fresh Vegetables', price: 3000, fillPercentage: 0 },
          { id: 4, name: 'Meat Package', price: 2000, fillPercentage: 0 }
        ],
        business: [
          { id: 5, name: 'Office Chair', price: 8000, fillPercentage: 0 },
          { id: 6, name: 'Standing Desk', price: 15000, fillPercentage: 0 },
          { id: 7, name: 'Monitor', price: 12000, fillPercentage: 0 }
        ],
        clothes: [
          { id: 8, name: 'Business Shirt', price: 2500, fillPercentage: 0 },
          { id: 9, name: 'Formal Pants', price: 3500, fillPercentage: 0 },
          { id: 10, name: 'Leather Shoes', price: 4000, fillPercentage: 0 }
        ],
        software: [
          { id: 11, name: 'Adobe Creative Suite', price: 2500, fillPercentage: 0 },
          { id: 12, name: 'Microsoft Office', price: 3000, fillPercentage: 0 },
          { id: 13, name: 'Design Software', price: 5000, fillPercentage: 0 }
        ]
      }
    }
  },
  mounted() {
    this.initializeSortable()
    this.updateBudgetVisualization()
    // Select first item in groceries by default
    if (this.getCurrentCategoryItems().length > 0) {
      this.selectedItem = this.getCurrentCategoryItems()[0]
    }
  },
  beforeUnmount() {
    // Clean up sortable instance when component is destroyed
    if (this.sortableInstance) {
      this.sortableInstance.destroy()
      this.sortableInstance = null
    }
  },
  methods: {
    selectCategory(categoryId) {
      this.selectedCategory = categoryId
      this.selectedItem = null
      this.$nextTick(() => {
        this.initializeSortable()
        this.updateBudgetVisualization()
      })
    },
    selectItem(item) {
      this.selectedItem = item
    },
    getCurrentCategoryName() {
      const category = this.categories.find(c => c.id === this.selectedCategory)
      return category ? category.name : ''
    },
    getCurrentCategoryItems() {
      return this.items[this.selectedCategory] || []
    },
    updateBudgetVisualization() {
      const items = this.getCurrentCategoryItems()
      let remainingBudget = this.budget
      
      items.forEach(item => {
        if (remainingBudget >= item.price) {
          item.fillPercentage = 100
          remainingBudget -= item.price
        } else if (remainingBudget > 0) {
          item.fillPercentage = (remainingBudget / item.price) * 100
          remainingBudget = 0
        } else {
          item.fillPercentage = 0
        }
      })
    },
    initializeSortable() {
      // Destroy existing sortable instance if it exists
      if (this.sortableInstance) {
        this.sortableInstance.destroy()
        this.sortableInstance = null
      }
      
      if (this.$refs.itemsList) {
        this.sortableInstance = Sortable.create(this.$refs.itemsList, {
          animation: 150,
          ghostClass: 'sortable-ghost',
          forceFallback: true,
          fallbackTolerance: 3,
          touchStartThreshold: 0,
          delay: 100,
          delayOnTouchStart: true,
          onEnd: (evt) => {
            const items = this.getCurrentCategoryItems()
            const item = items.splice(evt.oldIndex, 1)[0]
            items.splice(evt.newIndex, 0, item)
            this.updateBudgetVisualization()
          }
        })
      }
    },
    toggleAddForm() {
      this.showAddForm = !this.showAddForm
      if (this.showAddForm) {
        this.newItemName = ''
        this.newItemPrice = 0
      }
    },
    addNewItem() {
      if (this.newItemName.trim() && this.newItemPrice > 0) {
        const newItem = {
          id: Date.now(),
          name: this.newItemName.trim(),
          price: this.newItemPrice,
          fillPercentage: 0
        }
        this.items[this.selectedCategory].push(newItem)
        this.updateBudgetVisualization()
        this.cancelAddForm()
        this.$nextTick(() => {
          this.initializeSortable()
        })
      }
    },
    cancelAddForm() {
      this.showAddForm = false
      this.newItemName = ''
      this.newItemPrice = 0
    },
    handleBudgetInput(event) {
      const value = event.target.value.replace(/[^\d]/g, '')
      if (value) {
        this.budget = parseInt(value)
        this.updateBudgetVisualization()
      }
    }
  },
  computed: {
    budgetFormatted() {
      return '₱' + this.budget.toLocaleString()
    }
  },
  watch: {
    selectedCategory() {
      this.updateBudgetVisualization()
    }
  }
}
</script>
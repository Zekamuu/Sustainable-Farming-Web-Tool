# Saroja.earth Fertilizer Recommendation Tool - Technical Design Document

## 1. Project Overview

### 1.1 Purpose
The Saroja.earth Fertilizer Recommendation Tool is a web-based application designed to help farmers and agricultural professionals determine optimal fertilizer recommendations based on soil NPK (Nitrogen, Phosphorus, Potassium) analysis and farm size parameters.

### 1.2 Target Audience
- **Primary**: Indian farmers seeking soil health solutions
- **Secondary**: Agricultural researchers and soil testing professionals
- **Tertiary**: Agricultural consultants and extension workers

### 1.3 Key Features
- Real-time NPK analysis and fertilizer recommendations
- Multilingual support (English, Hindi, Punjabi)
- Farm size calculation with multiple unit conversions
- Responsive design for desktop and mobile devices
- Integration with Saroja.earth brand identity

## 2. Technical Architecture

### 2.1 Technology Stack
- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Styling**: Custom CSS with Google Fonts (Poppins, Open Sans)
- **Icons**: Font Awesome 6.0
- **Responsive Design**: CSS Grid and Flexbox
- **Browser Support**: Modern browsers (Chrome, Firefox, Safari, Edge)

### 2.2 File Structure
```
saroja-earth-compact.html
├── HTML Structure
├── CSS Styles (Embedded)
└── JavaScript Logic (Embedded)
```

### 2.3 Dependencies
- Google Fonts API (Poppins, Open Sans)
- Font Awesome CDN (Icons)
- No external JavaScript frameworks
- No backend dependencies

## 3. User Interface Design

### 3.1 Layout Architecture
```
┌─────────────────────────────────────────────────────────┐
│                    Header Section                       │
│  [Logo] [Navigation] [Language Selector]               │
├─────────────────────────────────────────────────────────┤
│  Input Panel          │  Results Panel                 │
│  ┌─────────────────┐  │  ┌─────────────────────────┐  │
│  │ NPK Sliders     │  │  │ Recommended Fertilizers │  │
│  │ Farm Size       │  │  │ Product Cards           │  │
│  └─────────────────┘  │  └─────────────────────────┘  │
├─────────────────────────────────────────────────────────┤
│              Collapsible Information Section            │
└─────────────────────────────────────────────────────────┘
```

### 3.2 Responsive Breakpoints
- **Desktop**: >1024px (Side-by-side layout)
- **Tablet**: 768px-1024px (Stacked layout)
- **Mobile**: <768px (Vertical stacking)

### 3.3 Color Palette
```css
Primary Colors:
- Deep Forest Green: #2c5530
- Natural Green: #4a7c59
- Light Green: #6b8e6b
- Accent Green: #8fb3a0

Text Colors:
- Primary Text: #2c3e50
- Secondary Text: #5a6c5d
- White: #ffffff

Background:
- Gradient: linear-gradient(135deg, #2c5530 0%, #4a7c59 50%, #6b8e6b 100%)
```

## 4. Component Specifications

### 4.1 Header Component
**Purpose**: Navigation and branding
**Elements**:
- Logo (Saroja.earth)
- Tagline (Soil Health & Fertilizer Recommendations)
- Navigation buttons (Soil Products, Crop Needs, BioChar Quiz, Soil Labs)
- Language selector (English, Hindi, Punjabi)

**Styling**:
- Background: Semi-transparent white with backdrop blur
- Border radius: 20px
- Shadow: Green-tinted shadow
- Typography: Poppins for logo, Open Sans for tagline

### 4.2 NPK Input Component
**Purpose**: Soil analysis input
**Elements**:
- Three range sliders (Nitrogen, Phosphorus, Potassium)
- Value displays
- Labels with multilingual support

**Technical Specifications**:
- Range: 0-200 kg/hectare
- Default values: 50 for all nutrients
- Real-time value updates
- Custom slider styling with green theme

### 4.3 Farm Size Component
**Purpose**: Farm area calculation
**Elements**:
- Number input field
- Unit selector dropdown
- Supported units: Hectares, Acres, Bigha, Marla, Kanal

**Conversion Logic**:
```javascript
const conversions = {
    'hectares': 1,
    'acres': 0.404686,
    'bigha': 0.1338,
    'marla': 0.0025,
    'kanal': 0.05
};
```

### 4.4 Product Recommendation Component
**Purpose**: Display fertilizer recommendations
**Elements**:
- Dynamic product cards
- Product specifications
- Pricing information
- Dosage calculations

**Data Structure**:
```javascript
const fertilizers = {
    topGrade: {
        name: { en: "Top Grade Soil Additive", hi: "टॉप ग्रेड मिट्टी योजक", pa: "ਟੌਪ ਗ੍ਰੇਡ ਮਿੱਟੀ ਐਡਿਟਿਵ" },
        composition: "Biochar: 25%, Compost: 40%, Mycorrhiza: 10%, N: 5%, P: 5%, K: 5%, Micro-nutrients: 5%, PSB: 5%",
        costPerKg: 222,
        description: "Premium soil additive with balanced NPK and beneficial microorganisms"
    }
    // ... additional fertilizer types
};
```

## 5. Algorithm Specifications

### 5.1 Recommendation Logic
```javascript
function getRecommendations(nitrogen, phosphorus, potassium) {
    const recommendations = [];
    
    // High nitrogen levels
    if (nitrogen >= 70) {
        recommendations.push(fertilizers.topGrade);
    }
    
    // High phosphorus levels
    if (phosphorus >= 70) {
        recommendations.push(fertilizers.pGrade);
    }
    
    // High potassium levels
    if (potassium >= 70) {
        recommendations.push(fertilizers.kGrade);
    }
    
    // Balanced NPK levels
    if (nitrogen >= 50 && phosphorus >= 50 && potassium >= 50) {
        recommendations.push(fertilizers.nGrade);
    }
    
    // Low nutrient levels
    if (nitrogen < 50 || phosphorus < 50 || potassium < 50) {
        recommendations.push(fertilizers.lowGrade);
    }
    
    return recommendations;
}
```

### 5.2 Dosage Calculation
```javascript
function calculateDosage(fertilizer) {
    const dosageMap = {
        topGrade: 50,    // kg per hectare
        lowGrade: 30,
        pGrade: 40,
        kGrade: 45,
        nGrade: 40
    };
    return dosageMap[fertilizer] || 35;
}
```

### 5.3 Cost Calculation
```javascript
function calculateTotalCost(dosagePerHectare, farmSizeInHectares, costPerKg) {
    const totalDosage = dosagePerHectare * farmSizeInHectares;
    return totalDosage * costPerKg;
}
```

## 6. Multilingual Implementation

### 6.1 Language Support
- **English**: Primary language
- **Hindi**: हिन्दी (Devanagari script)
- **Punjabi**: ਪੰਜਾਬੀ (Gurmukhi script)

### 6.2 Implementation Method
```javascript
// Data attributes for multilingual content
<div data-en="Soil Products" data-hi="मिट्टी उत्पाद" data-pa="ਮਿੱਟੀ ਉਤਪਾਦ">
    Soil Products
</div>

// Language switching function
function updateLanguage() {
    const selectedLanguage = languageSelect.value;
    document.querySelectorAll('[data-en]').forEach(element => {
        const text = element.getAttribute(`data-${selectedLanguage}`);
        if (text) {
            element.textContent = text;
        }
    });
}
```

## 7. Performance Considerations

### 7.1 Optimization Strategies
- **CSS**: Embedded styles to reduce HTTP requests
- **JavaScript**: Vanilla JS for minimal overhead
- **Fonts**: Google Fonts with display=swap for performance
- **Images**: No external images, using CSS gradients
- **Animations**: CSS transitions for smooth interactions

### 7.2 Loading Performance
- **Initial Load**: ~50KB total file size
- **Font Loading**: Asynchronous with fallbacks
- **No External Dependencies**: Self-contained application

## 8. Accessibility Features

### 8.1 WCAG Compliance
- **Keyboard Navigation**: Full keyboard accessibility
- **Screen Reader Support**: Semantic HTML structure
- **Color Contrast**: High contrast ratios for readability
- **Focus Indicators**: Visible focus states

### 8.2 Responsive Design
- **Mobile-First**: Optimized for mobile devices
- **Touch-Friendly**: Appropriate touch targets (44px minimum)
- **Flexible Layout**: Adapts to various screen sizes

## 9. Browser Compatibility

### 9.1 Supported Browsers
- **Chrome**: 80+
- **Firefox**: 75+
- **Safari**: 13+
- **Edge**: 80+

### 9.2 Feature Support
- **CSS Grid**: Modern browsers
- **CSS Custom Properties**: Fallbacks provided
- **ES6 JavaScript**: Transpiled for older browsers

## 10. Security Considerations

### 10.1 Client-Side Security
- **No User Data Storage**: No sensitive data persistence
- **Input Validation**: Client-side validation for form inputs
- **XSS Prevention**: No dynamic content injection

### 10.2 Data Privacy
- **No Data Collection**: No user data transmitted
- **Local Processing**: All calculations performed client-side
- **No Cookies**: No tracking or analytics

## 11. Testing Strategy

### 11.1 Functional Testing
- **NPK Slider Functionality**: Value updates and recommendations
- **Language Switching**: All text elements update correctly
- **Farm Size Calculations**: Accurate conversions and cost calculations
- **Responsive Design**: Layout adaptation across devices

### 11.2 Cross-Browser Testing
- **Desktop Browsers**: Chrome, Firefox, Safari, Edge
- **Mobile Browsers**: iOS Safari, Chrome Mobile
- **Tablet Testing**: iPad, Android tablets

## 12. Deployment Considerations

### 12.1 Hosting Requirements
- **Static Hosting**: No server-side requirements
- **HTTPS**: Recommended for production
- **CDN**: Optional for global performance

### 12.2 Integration Points
- **Saroja.earth Website**: Can be embedded as iframe
- **Standalone Deployment**: Self-contained HTML file
- **Mobile App**: Can be wrapped in WebView

## 13. Future Enhancements

### 13.1 Planned Features
- **Soil Test Upload**: CSV/Excel file import
- **Historical Data**: Save and compare recommendations
- **Advanced Algorithms**: Machine learning-based recommendations
- **Offline Support**: Service worker implementation

### 13.2 Scalability Considerations
- **Modular Architecture**: Easy to extend with new features
- **API Integration**: Ready for backend integration
- **Database Integration**: Prepared for data persistence

## 14. Maintenance and Support

### 14.1 Code Organization
- **Modular JavaScript**: Separated functions for maintainability
- **CSS Organization**: Logical grouping of styles
- **Documentation**: Inline comments for complex logic

### 14.2 Update Procedures
- **Version Control**: Git-based versioning
- **Testing Protocol**: Automated testing before deployment
- **Rollback Plan**: Previous version restoration capability

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Author**: Technical Team  
**Review Status**: Approved

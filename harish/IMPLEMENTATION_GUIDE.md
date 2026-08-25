# Leads Module - Implementation Guide

## Overview
This guide shows you how to implement the **Add Lead**, **Update Lead**, and **Delete Lead** functionality for your CRM portal.

---

## File Structure & Update Plan

### FRONTEND FILES

#### 1. **NEW FILE - Add LeadForm Component**
**Path:** `frontend/src/pages/leads/LeadForm.jsx`

This is a reusable modal form component for creating and editing leads. It includes:
- Form validation (first name, last name, email format)
- All lead fields (firstName, lastName, email, phone, company, source, status, notes)
- Error handling and display
- Loading state management
- Status dropdown with all lead statuses

**File:** `LeadForm.jsx` (provided)

---

#### 2. **UPDATE - Leads Page**
**Path:** `frontend/src/pages/leads/Leads.jsx`

Replace the existing file with the updated version that includes:
- Modal state management (isFormOpen, selectedLead)
- Form submission handlers (create, update, delete)
- Edit button in the table rows
- Delete button in the table rows with confirmation dialog
- "Add Lead" button integration
- Error handling and display
- Action buttons column in the data table

**File:** `Leads.jsx` (provided)

---

### BACKEND FILES

✅ **Already Complete** - No backend changes needed!

Your backend already has:
- ✅ `LeadController.java` - All endpoints (GET, POST, PUT, DELETE)
- ✅ `LeadService.java` - Create, Read, Update, Delete logic
- ✅ `LeadRepository.java` - Database queries
- ✅ `LeadDto.java` - Data transfer object
- ✅ `LeadRequest.java` - Request validation
- ✅ `LeadMapper.java` - Entity to DTO mapping
- ✅ `LeadSpecification.java` - Advanced filtering
- ✅ `Lead.java` - Entity with all fields

---

### DATABASE SCHEMA

✅ **Already Complete** - No database changes needed!

Your database already has the `leads` table with all required fields:
- id
- first_name
- last_name
- email
- phone
- company
- source
- status (ENUM: NEW, CONTACTED, QUALIFIED, CONVERTED, LOST)
- owner_id
- converted_contact_id
- converted_at
- notes
- created_at
- updated_at

---

## API Endpoints Available

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/leads` | List all leads with filters |
| GET | `/api/leads/{id}` | Get single lead |
| POST | `/api/leads` | Create new lead |
| PUT | `/api/leads/{id}` | Update lead |
| DELETE | `/api/leads/{id}` | Delete lead |
| POST | `/api/leads/{id}/convert` | Convert lead to contact |

---

## Required CSS Classes

Add these CSS classes to your stylesheet if they don't exist:

```css
/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
  max-width: 600px;
  width: 90%;
  max-height: 90vh;
  overflow-y: auto;
  padding: 24px;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  border-bottom: 1px solid #e5e7eb;
  padding-bottom: 16px;
}

.modal-header h3 {
  margin: 0;
  font-size: 20px;
  font-weight: 600;
}

.close-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  color: #6b7280;
  transition: color 0.2s;
}

.close-button:hover {
  color: #000;
}

.close-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 24px;
  border-top: 1px solid #e5e7eb;
  padding-top: 16px;
}

/* Form Styles */
.form-group {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 8px;
  font-weight: 500;
  color: #374151;
  font-size: 14px;
}

.form-group input,
.form-group select,
.form-group textarea {
  padding: 10px 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 14px;
  font-family: inherit;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.form-group input:disabled,
.form-group select:disabled,
.form-group textarea:disabled {
  background-color: #f9fafb;
  color: #9ca3af;
  cursor: not-allowed;
}

.form-group input.input-error {
  border-color: #ef4444;
}

.form-group textarea {
  resize: vertical;
  min-height: 100px;
}

.error-text {
  color: #ef4444;
  font-size: 13px;
  margin-top: 4px;
}

/* Action Buttons */
.action-buttons {
  display: flex;
  gap: 8px;
}

.icon-button {
  background: none;
  border: 1px solid #e5e7eb;
  padding: 8px;
  border-radius: 6px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  color: #6b7280;
}

.icon-button:hover {
  border-color: #9ca3af;
  color: #374151;
}

.icon-button.edit-button:hover {
  background-color: #eff6ff;
  border-color: #3b82f6;
  color: #3b82f6;
}

.icon-button.delete-button:hover {
  background-color: #fef2f2;
  border-color: #ef4444;
  color: #ef4444;
}

/* Alert Styles */
.alert {
  padding: 12px 16px;
  border-radius: 6px;
  margin-bottom: 16px;
  font-size: 14px;
}

.alert-error {
  background-color: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
}

.alert-success {
  background-color: #f0fdf4;
  border: 1px solid #bbf7d0;
  color: #166534;
}

/* Status Badge */
.status-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 13px;
  font-weight: 500;
}

.status-badge {
  background-color: #f3f4f6;
  color: #374151;
}

/* Override for specific statuses if using a formatter */
.status-badge.new { background-color: #e0e7ff; color: #3730a3; }
.status-badge.contacted { background-color: #dbeafe; color: #075985; }
.status-badge.qualified { background-color: #dbeafe; color: #075985; }
.status-badge.converted { background-color: #dcfce7; color: #166534; }
.status-badge.lost { background-color: #fecaca; color: #991b1b; }
```

---

## Installation Steps

### Step 1: Add LeadForm Component
1. Create a new file: `frontend/src/pages/leads/LeadForm.jsx`
2. Copy the content from the provided `LeadForm.jsx`

### Step 2: Update Leads Page
1. Replace the existing file: `frontend/src/pages/leads/Leads.jsx`
2. Copy the content from the provided `Leads.jsx`

### Step 3: Add CSS Classes
1. Add the CSS classes above to your stylesheet (usually `frontend/src/styles/` or `index.css`)
2. Adjust colors and spacing to match your theme if needed

### Step 4: Verify Backend Service
The `leadService.js` is already complete with all methods:
- `list()` - Fetch all leads
- `get(id)` - Fetch single lead
- `create(data)` - Create new lead
- `update(id, data)` - Update existing lead
- `remove(id)` - Delete lead
- `convert(id)` - Convert lead to contact

---

## Features Implemented

### Add Lead
- Click "Add Lead" button
- Fill in lead details (first name, last name, email, phone, company, source, status, notes)
- Form validation (required fields and email format)
- Submit to create new lead
- Automatically added to the table

### Update Lead
- Click the edit icon (pencil) on any lead row
- Modal opens with pre-filled lead data
- Modify any field
- Submit to update the lead
- Table refreshes with updated data

### Delete Lead
- Click the delete icon (trash) on any lead row
- Confirmation dialog appears
- If confirmed, lead is deleted from the database
- Automatically removed from the table

### Additional Features
- Real-time search/filter on lead name and email
- Status badge display
- Error handling and user feedback
- Loading states during operations
- Form validation with error messages

---

## Testing Checklist

- [ ] Can add a new lead
- [ ] Can edit an existing lead
- [ ] Can delete a lead with confirmation
- [ ] Search/filter works on name and email
- [ ] Form validation works (first name, last name required)
- [ ] Email format validation works
- [ ] Status dropdown shows all statuses
- [ ] Error messages display correctly
- [ ] Modal closes on successful submission
- [ ] Modal closes when clicking Cancel
- [ ] All CRUD operations update the table immediately

---

## Common Issues & Solutions

### Issue: Modal doesn't appear
- **Solution:** Ensure `LeadForm.jsx` is in the correct path and imported correctly

### Issue: Form submission fails
- **Solution:** Check browser console for API errors. Verify backend is running and accessible.

### Issue: CSS doesn't match
- **Solution:** Add the provided CSS classes to your stylesheet. Adjust colors to match your theme.

### Issue: Import errors
- **Solution:** Verify all imports are correct:
  - `import LeadForm from "./LeadForm";` (should be in same directory as Leads.jsx)
  - Icons from `lucide-react` should be available

---

## Next Steps (After JWT Implementation)

When you implement JWT and password hashing:
1. The API will automatically handle authentication
2. No changes needed in the frontend components
3. Ensure your API calls include the authorization header (leadService.js already handles this via the api client)

---

## File Summary

| File | Type | Action | Path |
|------|------|--------|------|
| LeadForm.jsx | Component | **CREATE** | `frontend/src/pages/leads/LeadForm.jsx` |
| Leads.jsx | Page | **UPDATE** | `frontend/src/pages/leads/Leads.jsx` |
| leadService.js | Service | No change | `frontend/src/services/leadService.js` ✅ |
| Backend | All files | No change | `backend/src/main/java/com/crm/portal/...` ✅ |
| Database | Schema | No change | `database/schema.sql` ✅ |

---

## Support

If you need further adjustments:
1. Check the browser console (F12) for error messages
2. Check the backend logs for API errors
3. Verify all file paths match your project structure
4. Ensure CSS classes are properly defined in your stylesheet

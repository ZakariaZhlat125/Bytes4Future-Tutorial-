# React.js Session 4: Forms and useEffect

**Duration:** 3 hours  
**Level:** Beginner  
**Prerequisites:** Session 1-3 completed (Components, Props, Events, State, useState, Conditional Rendering)

---

## Session Timeline

### Part 1: Forms (1 hour 30 minutes)
- **0:00-0:05:** HTML Forms vs React Forms, Controlled Components (Topics 1-2)
- **0:05-0:10:** Uncontrolled Components (Topic 3)
- **0:10-0:15:** Input State, Text Input (Topics 4-5)
- **0:15-0:20:** Number Input, Checkbox (Topics 6-7)
- **0:20-0:25:** Radio, Select (Topics 8-9)
- **0:25-0:30:** Textarea, Multiple inputs (Topics 10-11)
- **0:30-0:35:** Form submit, preventDefault (Topics 12-13)
- **0:35-0:40:** Form state, Validation (Topics 14-15)
- **0:40-0:45:** Error messages, Reset form (Topics 16-17)
- **0:45-1:30:** Forms Practice Exercises

### Part 2: Rendering vs Event vs Side Effect (15 minutes)
- **1:30-1:45:** Understanding the differences with examples

### Part 3: useEffect (1 hour 15 minutes)
- **1:45-1:50:** What is useEffect, Why useEffect (Topics 1-2)
- **1:50-1:55:** Side Effects (Topic 3)
- **1:55-2:00:** useEffect syntax (Topic 4)
- **2:00-2:05:** Empty dependency array (Topic 5)
- **2:05-2:10:** Dependencies (Topic 6)
- **2:10-2:15:** Effect execution (Topic 7)
- **2:20-2:25:** Cleanup (Topic 8)
- **2:25-2:30:** setInterval (Topic 9)
- **2:30-2:35:** Event listeners (Topic 10)
- **2:35-2:40:** Common mistakes, Infinite loops (Topics 11-12)
- **2:40-2:45:** Missing dependencies, Unnecessary useEffect (Topics 13-14)
- **2:45-2:50:** When NOT to use useEffect (Topic 15)
- **2:50-3:00:** useEffect Practice Exercises

---

## Part 1: Forms

### 1. HTML Forms vs React Forms

**What is it?**
Understanding the fundamental differences between traditional HTML forms and React forms, and how React's approach changes form handling.

**Why we need it?**
- React forms work differently than HTML forms
- Understanding the difference prevents confusion
- React provides more control over form behavior
- Enables validation and dynamic form behavior
- Better user experience

**How it works:**
- HTML forms: Data handled by browser, page reloads on submit
- React forms: Data controlled by React state, no page reload
- HTML forms: DOM contains the current values
- React forms: State contains the current values, DOM reflects state

**Syntax Comparison:**

**HTML Form:**
```html
<form action="/submit" method="POST">
  <input type="text" name="username" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</form>
```

**React Form:**
```tsx
function ReactForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: ''
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text" 
        value={formData.username}
        onChange={(e) => setFormData({...formData, username: e.target.value})}
      />
      <input 
        type="email" 
        value={formData.email}
        onChange={(e) => setFormData({...formData, email: e.target.value})}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Key Differences:**

| Aspect | HTML Forms | React Forms |
|--------|-----------|-------------|
| Data Storage | DOM elements | React state |
| Updates | Direct DOM manipulation | State updates trigger re-renders |
| Submit Behavior | Page reload/no reload controlled by HTML | Prevented by React, handled in JavaScript |
| Validation | HTML5 validation or JavaScript | Custom validation with state |
| Access | JavaScript API or form submission | React state |

**Simple Example:**

**HTML Form (Traditional):**
```html
<form onsubmit="handleFormSubmit(event)">
  <input type="text" name="username" id="username" />
  <input type="email" name="email" id="email" />
  <button type="submit">Submit</button>
</form>

<script>
function handleFormSubmit(event) {
  event.preventDefault();
  const username = document.getElementById('username').value;
  const email = document.getElementById('email').value;
  console.log({ username, email });
}
</script>
```

**React Form (Controlled):**
```tsx
function MyForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: ''
  });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="username"
        value={formData.username}
        onChange={handleChange}
        placeholder="Username"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Real-world Example:**

**HTML Form (User Registration):**
```html
<form action="/register" method="POST">
  <input type="text" name="name" required />
  <input type="email" name="email" required />
  <input type="password" name="password" required minlength="8" />
  <input type="password" name="confirmPassword" required />
  <button type="submit">Register</button>
</form>
```

**React Form (User Registration with Validation):**
```tsx
interface FormData {
  name: string;
  email: string;
  password: string;
  confirmPassword: string;
}

interface Errors {
  name?: string;
  email?: string;
  password?: string;
  confirmPassword?: string;
}

function RegistrationForm() {
  const [formData, setFormData] = useState<FormData>({
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
  });

  const [errors, setErrors] = useState<Errors>({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const validateForm = (): boolean => {
    const newErrors: Errors = {};

    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }

    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 8) {
      newErrors.password = 'Password must be at least 8 characters';
    }

    if (formData.password !== formData.confirmPassword) {
      newErrors.confirmPassword = 'Passwords do not match';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    // Clear error when user starts typing
    if (errors[name as keyof Errors]) {
      setErrors(prev => ({ ...prev, [name]: undefined }));
    }
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) return;

    setIsSubmitting(true);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Registration successful:', formData);
      alert('Registration successful!');
    } catch (error) {
      console.error('Registration failed:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="registration-form">
      <div className="form-group">
        <label htmlFor="name">Name</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          className={errors.name ? 'error' : ''}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="email">Email</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="password">Password</label>
        <input
          type="password"
          id="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          className={errors.password ? 'error' : ''}
        />
        {errors.password && <span className="error-message">{errors.password}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="confirmPassword">Confirm Password</label>
        <input
          type="password"
          id="confirmPassword"
          name="confirmPassword"
          value={formData.confirmPassword}
          onChange={handleChange}
          className={errors.confirmPassword ? 'error' : ''}
        />
        {errors.confirmPassword && <span className="error-message">{errors.confirmPassword}</span>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Registering...' : 'Register'}
      </button>
    </form>
  );
}
```

**Common Errors:**
- Mixing HTML form behavior with React forms
- Not preventing default form submission
- Using HTML form attributes with React controlled components
- Not understanding when to use controlled vs uncontrolled

**Best Practices:**
- Use controlled components for most forms
- Always prevent default in React form handlers
- Implement proper validation
- Provide clear error messages
- Handle loading and error states

---

### 2. Controlled Components

**What is it?**
Controlled components are form elements where React controls the value, and changes are handled through React state and event handlers.

**Why we need it?**
- React has complete control over form data
- Enables real-time validation
- Provides consistent state management
- Easier to implement complex form behavior
- Better user experience

**How it works:**
- Form value stored in React state
- Input value prop set to state value
- onChange handler updates state
- React re-renders with new state
- Form always reflects current state

**Syntax:**
```tsx
function ControlledInput() {
  const [value, setValue] = useState('');
  
  return (
    <input 
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

**Simple Example:**
```tsx
function SearchBox() {
  const [searchTerm, setSearchTerm] = useState('');
  
  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      <p>Searching for: {searchTerm}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface ContactFormProps {
  onSubmit: (data: ContactFormData) => void;
}

interface ContactFormData {
  name: string;
  email: string;
  subject: string;
  message: string;
}

function ContactForm({ onSubmit }: ContactFormProps) {
  const [formData, setFormData] = useState<ContactFormData>({
    name: '',
    email: '',
    subject: '',
    message: ''
  });

  const [touched, setTouched] = useState<Record<keyof ContactFormData, boolean>>({
    name: false,
    email: false,
    subject: false,
    message: false
  });

  const [errors, setErrors] = useState<Partial<Record<keyof ContactFormData, string>>>({});

  const validateField = (field: keyof ContactFormData, value: string): string | undefined => {
    switch (field) {
      case 'name':
        return value.trim() ? undefined : 'Name is required';
      case 'email':
        return !/\S+@\S+\.\S+/.test(value) ? 'Invalid email' : undefined;
      case 'subject':
        return value.trim() ? undefined : 'Subject is required';
      case 'message':
        return value.trim().length >= 10 ? undefined : 'Message must be at least 10 characters';
      default:
        return undefined;
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Validate on change if field has been touched
    if (touched[name]) {
      const error = validateField(name as keyof ContactFormData, value);
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name as keyof ContactFormData, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    // Validate all fields
    const newErrors: Partial<Record<keyof ContactFormData, string>> = {};
    let isValid = true;

    (Object.keys(formData) as Array<keyof ContactFormData>).forEach(field => {
      const error = validateField(field, formData[field]);
      if (error) {
        newErrors[field] = error;
        isValid = false;
      }
    });

    setErrors(newErrors);
    setTouched({
      name: true,
      email: true,
      subject: true,
      message: true
    });

    if (isValid) {
      onSubmit(formData);
    }
  };

  const isFormValid = Object.keys(errors).length === 0 && 
    Object.values(touched).every(Boolean) &&
    Object.values(formData).every(value => value.trim().length > 0);

  return (
    <form onSubmit={handleSubmit} className="contact-form">
      <div className="form-group">
        <label htmlFor="name">Name</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.name ? 'error' : ''}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="email">Email</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="subject">Subject</label>
        <input
          type="text"
          id="subject"
          name="subject"
          value={formData.subject}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.subject ? 'error' : ''}
        />
        {errors.subject && <span className="error-message">{errors.subject}</span>}
      </div>

      <div className="form-group">
        <label htmlFor="message">Message</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          onBlur={handleBlur}
          rows={5}
          className={errors.message ? 'error' : ''}
        />
        {errors.message && <span className="error-message">{errors.message}</span>}
      </div>

      <button type="submit" disabled={!isFormValid}>
        Send Message
      </button>
    </form>
  );
}
```

**Controlled Component Benefits:**
- Real-time validation
- Instant feedback
- Consistent state management
- Easy to implement complex logic
- Better user experience

**Common Errors:**
- Not providing value prop
- Not handling onChange events
- Mixing controlled and uncontrolled approaches
- Not handling edge cases (empty values, etc.)

**Best Practices:**
- Always provide value prop
- Handle onChange events properly
- Implement validation
- Provide user feedback
- Handle loading and error states

---

### 3. Uncontrolled Components (Brief Overview)

**What is it?**
Uncontrolled components are form elements that maintain their own internal state, similar to traditional HTML forms, and values are accessed using refs rather than React state.

**Why we need it:**
- Useful for simple forms
- Can be more performant for large forms
- Integration with non-React code
- Familiar HTML form behavior
- Less boilerplate for simple cases

**How it works:**
- Form data stored in DOM
- Accessed using useRef
- No onChange handlers needed
- Similar to traditional HTML forms
- Use refs to get values

**Syntax:**
```tsx
function UncontrolledForm() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log(inputRef.current?.value);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} defaultValue="initial value" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Simple Example:**
```tsx
function SimpleForm() {
  const nameRef = useRef<HTMLInputElement>(null);
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    alert(`Hello, ${nameRef.current?.value}!`);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} defaultValue="John" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Real-world Example:**
```tsx
function FileUploadForm() {
  const fileInputRef = useRef<HTMLInputElement>(null);
  const [uploadedFile, setUploadedFile] = useState<File | null>(null);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const file = fileInputRef.current?.files?.[0];
    if (file) {
      setUploadedFile(file);
      console.log('File uploaded:', file.name);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={fileInputRef}
        type="file"
        accept="image/*"
      />
      <button type="submit">Upload</button>
      {uploadedFile && <p>Uploaded: {uploadedFile.name}</p>}
    </form>
  );
}
```

**When to Use Uncontrolled:**
- Simple forms with minimal validation
- File uploads
- Integration with third-party libraries
- Performance-critical large forms
- Quick prototypes

**Controlled vs Uncontrolled:**

| Aspect | Controlled | Uncontrolled |
|--------|-----------|---------------|
| Data Source | React state | DOM |
| Updates | React re-renders | Direct DOM manipulation |
| Validation | Easy to implement | Harder to implement |
| Real-time feedback | Excellent | Limited |
| Best For | Most forms | Simple cases, file uploads |

**Common Errors:**
- Using uncontrolled when controlled would be better
- Not understanding when to use refs
- Mixing controlled and uncontrolled
- Not handling null ref values

**Best Practices:**
- Use controlled components for most forms
- Use uncontrolled for file uploads and simple cases
- Understand trade-offs between approaches
- Use refs correctly
- Consider user experience

---

### 4. Input State

**What is it?**
The React state that holds the current value of form inputs, enabling controlled components to track and manage user input.

**Why we need it:**
- React needs to know input values
- Enables validation and feedback
- Controls form behavior
- Provides single source of truth
- Enables real-time updates

**How it works:**
- State initialized with default value
- Input value prop set to state
- onChange handler updates state
- React re-renders with new state
- Form reflects current state

**Syntax:**
```tsx
function InputWithState() {
  const [inputValue, setInputValue] = useState('');
  
  return (
    <input 
      value={inputValue}
      onChange={(e) => setInputValue(e.target.value)}
    />
  );
}
```

**Simple Example:**
```tsx
function NameInput() {
  const [name, setName] = useState('');
  
  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <p>Hello, {name || 'stranger'}!</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface FormState {
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
}

function ContactForm() {
  const [formState, setFormState] = useState<FormState>({
    firstName: '',
    lastName: '',
    email: '',
    phone: ''
  });

  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormState(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const resetForm = () => {
    setFormState({
      firstName: '',
      lastName: '',
      email: '',
      phone: ''
    });
  };

  const hasContent = Object.values(formState).some(value => value.trim().length > 0);

  return (
    <form className="contact-form">
      <div className="form-row">
        <div className="form-group">
          <label>First Name</label>
          <input
            type="text"
            name="firstName"
            value={formState.firstName}
            onChange={handleInputChange}
          />
        </div>
        <div className="form-group">
          <label>Last Name</label>
          <input
            type="text"
            name="lastName"
            value={formState.lastName}
            onChange={handleInputChange}
          />
        </div>
      </div>

      <div className="form-group">
        <label>Email</label>
        <input
          type="email"
          name="email"
          value={formState.email}
          onChange={handleInputChange}
        />
      </div>

      <div className="form-group">
        <label>Phone</label>
        <input
          type="tel"
          name="phone"
          value={formState.phone}
          onChange={handleInputChange}
        />
      </div>

      <div className="form-actions">
        <button type="button" onClick={resetForm} disabled={!hasContent}>
          Reset
        </button>
        <button type="submit">Submit</button>
      </div>
    </form>
  );
}
```

**Input State Patterns:**

1. **Single input:**
```tsx
const [value, setValue] = useState('');
```

2. **Multiple inputs in object:**
```tsx
const [formData, setFormData] = useState({
  field1: '',
  field2: '',
  field3: ''
});
```

3. **Array of inputs:**
```tsx
const [items, setItems] = useState(['', '', '']);
```

4. **Nested object state:**
```tsx
const [user, setUser] = useState({
  profile: { name: '', email: '' },
  settings: { theme: 'light' }
});
```

**Common Errors:**
- Not initializing state properly
- Not handling empty values
- Complex state structures when simple would work
- Not handling input validation in state

**Best Practices:**
- Initialize with appropriate default values
- Keep state structure simple
- Use TypeScript interfaces
- Handle edge cases (empty, null, undefined)
- Validate input values

---

### 5. Text Input

**What is it?**
Text input fields for single-line text entry, the most common form element for capturing user input.

**Why we need it:**
- Capture names, emails, search queries
- Most common form input type
- Foundation for user input
- Versatile and widely used
- Standard user experience

**How it works:**
- Controlled component with value prop
- onChange handler updates state
- Supports various text types
- Can have constraints (maxlength, pattern)
- Placeholder for user guidance

**Syntax:**
```tsx
<input
  type="text"
  value={stateValue}
  onChange={(e) => setStateValue(e.target.value)}
  placeholder="Enter text"
/>
```

**Simple Example:**
```tsx
function TextInput() {
  const [text, setText] = useState('');
  
  return (
    <div>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
      <p>You typed: {text}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface TextInputProps {
  label: string;
  name: string;
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  required?: boolean;
  maxLength?: number;
  disabled?: boolean;
  error?: string;
}

function TextInput({
  label,
  name,
  value,
  onChange,
  placeholder,
  required = false,
  maxLength,
  disabled = false,
  error
}: TextInputProps) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newValue = e.target.value;
    
    // Enforce maxLength
    if (maxLength && newValue.length > maxLength) {
      return;
    }
    
    onChange(newValue);
  };

  const characterCount = value.length;
  const remainingChars = maxLength ? maxLength - characterCount : null;

  return (
    <div className="text-input-group">
      <label htmlFor={name}>
        {label}
        {required && <span className="required">*</span>}
      </label>
      <input
        type="text"
        id={name}
        name={name}
        value={value}
        onChange={handleChange}
        placeholder={placeholder}
        required={required}
        maxLength={maxLength}
        disabled={disabled}
        className={error ? 'error' : ''}
      />
      {maxLength && (
        <div className="character-count">
          <span className={characterCount > maxLength * 0.9 ? 'warning' : ''}>
            {characterCount}
          </span>
          / {maxLength}
        </div>
      )}
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// Usage
function ProfileForm() {
  const [username, setUsername] = useState('');
  const [bio, setBio] = useState('');
  const [location, setLocation] = useState('');

  return (
    <form>
      <TextInput
        label="Username"
        name="username"
        value={username}
        onChange={setUsername}
        placeholder="Choose a username"
        required
        maxLength={20}
      />
      
      <TextInput
        label="Bio"
        name="bio"
        value={bio}
        onChange={setBio}
        placeholder="Tell us about yourself"
        maxLength={150}
      />
      
      <TextInput
        label="Location"
        name="location"
        value={location}
        onChange={setLocation}
        placeholder="Your city"
      />
    </form>
  );
}
```

**Text Input Types:**
- `text`: Standard text input
- `email`: Email validation
- `password`: Masked input
- `tel`: Telephone number
- `url`: URL validation
- `search`: Search interface

**Common Errors:**
- Not handling maxLength properly
- Not providing unique id and name
- Not implementing proper validation
- Not handling disabled state
- Not providing helpful placeholders

**Best Practices:**
- Always use controlled components
- Provide helpful placeholders
- Implement appropriate validation
- Use proper input types
- Handle disabled state properly
- Provide character count for limited inputs

---

### 6. Number Input

**What is it?**
Number input fields for numeric data entry, providing built-in validation and specialized mobile keyboards.

**Why we need it:**
- Capture numeric data (age, quantity, price)
- Built-in validation
- Specialized mobile keyboards
- Step controls for increment/decrement
- Range constraints

**How it works:**
- Controlled component with number state
- onChange handler updates state
- Supports min, max, step attributes
- Validates numeric input
- Can be disabled or read-only

**Syntax:**
```tsx
<input
  type="number"
  value={numericValue}
  onChange={(e) => setNumericValue(Number(e.target.value))}
  min={minValue}
  max={maxValue}
/>
```

**Simple Example:**
```tsx
function AgeInput() {
  const [age, setAge] = useState(0);
  
  return (
    <div>
      <input
        type="number"
        value={age}
        onChange={(e) => setAge(Number(e.target.value))}
        min={0}
        max={120}
      />
      <p>Your age: {age}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface NumberInputProps {
  label: string;
  name: string;
  value: number;
  onChange: (value: number) => void;
  min?: number;
  max?: number;
  step?: number;
  placeholder?: string;
  disabled?: boolean;
  error?: string;
}

function NumberInput({
  label,
  name,
  value,
  onChange,
  min,
  max,
  step = 1,
  placeholder,
  disabled = false,
  error
}: NumberInputProps) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newValue = Number(e.target.value);
    
    // Validate min/max
    if (min !== undefined && newValue < min) return;
    if (max !== undefined && newValue > max) return;
    
    onChange(newValue);
  };

  const isValid = !isNaN(value);
  const isBelowMin = min !== undefined && value < min;
  const isAboveMax = max !== undefined && value > max;

  return (
    <div className="number-input-group">
      <label htmlFor={name}>{label}</label>
      <input
        type="number"
        id={name}
        name={name}
        value={value}
        onChange={handleChange}
        min={min}
        max={max}
        step={step}
        placeholder={placeholder}
        disabled={disabled}
        className={error || !isValid ? 'error' : ''}
      />
      {min !== undefined && max !== undefined && (
        <div className="range-indicator">
          <span className={isBelowMin ? 'out-of-range' : ''}>
            Min: {min}
          </span>
          <span className={isAboveMax ? 'out-of-range' : ''}>
            Max: {max}
          </span>
        </div>
      )}
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// Usage
function ProductForm() {
  const [quantity, setQuantity] = useState(1);
  const [price, setPrice] = useState(0);
  const [discount, setDiscount] = useState(0);

  const totalPrice = quantity * price * (1 - discount / 100);

  return (
    <form>
      <NumberInput
        label="Quantity"
        name="quantity"
        value={quantity}
        onChange={setQuantity}
        min={1}
        max={100}
        step={1}
      />
      
      <NumberInput
        label="Price"
        name="price"
        value={price}
        onChange={setPrice}
        min={0}
        step={0.01}
        placeholder="0.00"
      />
      
      <NumberInput
        label="Discount (%)"
        name="discount"
        value={discount}
        onChange={setDiscount}
        min={0}
        max={100}
        step={1}
      />
      
      <div className="total">
        <strong>Total: ${totalPrice.toFixed(2)}</strong>
      </div>
    </form>
  );
}
```

**Number Input Features:**
- `min`: Minimum value
- `max`: Maximum value
- `step`: Increment/decrement amount
- `placeholder`: Placeholder text
- Mobile numeric keyboard

**Common Errors:**
- Not handling NaN values
- Not validating min/max constraints
- Using string state instead of number
- Not handling step increments properly
- Not providing appropriate decimal precision

**Best Practices:**
- Use number state type
- Validate min/max constraints
- Handle NaN and edge cases
- Provide appropriate step values
- Format displayed values appropriately
- Consider decimal precision for prices

---

### 7. Checkbox

**What is it?**
Checkbox input for boolean values (true/false), used for selections, toggles, and multiple-choice options.

**Why we need it?**
- Binary choices (yes/no)
- Multiple selections
- Feature toggles
- Terms and conditions
- Preferences and settings

**How it works:**
- Boolean state (true/false)
- checked prop set to state
- onChange toggles state
- Can be grouped with same name
- Supports indeterminate state

**Syntax:**
```tsx
<input
  type="checkbox"
  checked={booleanValue}
  onChange={(e) => setBooleanValue(e.target.checked)}
/>
```

**Simple Example:**
```tsx
function TermsCheckbox() {
  const [agreed, setAgreed] = useState(false);
  
  return (
    <div>
      <label>
        <input
          type="checkbox"
          checked={agreed}
          onChange={(e) => setAgreed(e.target.checked)}
        />
        I agree to the terms and conditions
      </label>
      <button disabled={!agreed}>Submit</button>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface CheckboxProps {
  label: string;
  name: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
  disabled?: boolean;
  indeterminate?: boolean;
}

function Checkbox({ label, name, checked, onChange, disabled = false, indeterminate = false }: CheckboxProps) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    onChange(e.target.checked);
  };

  return (
    <label className={`checkbox ${disabled ? 'disabled' : ''}`}>
      <input
        type="checkbox"
        name={name}
        checked={checked}
        onChange={handleChange}
        disabled={disabled}
        ref={(input) => {
          if (input) {
            input.indeterminate = indeterminate;
          }
        }}
      />
      <span className="checkbox-label">{label}</span>
    </label>
  );
}

// Usage
function SettingsForm() {
  const [settings, setSettings] = useState({
    notifications: true,
    emailUpdates: false,
    darkMode: false,
    analytics: true,
    marketing: false
  });

  const [selectAll, setSelectAll] = useState(false);

  const handleSettingChange = (setting: keyof typeof settings) => {
    setSettings(prev => ({
      ...prev,
      [setting]: !prev[setting]
    }));
  };

  const handleSelectAll = () => {
    const newValue = !selectAll;
    setSelectAll(newValue);
    setSettings({
      notifications: newValue,
      emailUpdates: newValue,
      darkMode: newValue,
      analytics: newValue,
      marketing: newValue
    });
  };

  const allSelected = Object.values(settings).every(Boolean);
  const someSelected = Object.values(settings).some(Boolean);

  return (
    <form className="settings-form">
      <div className="select-all">
        <Checkbox
          label="Select All"
          name="selectAll"
          checked={allSelected}
          indeterminate={someSelected && !allSelected}
          onChange={handleSelectAll}
        />
      </div>

      <div className="settings-list">
        <Checkbox
          label="Enable Notifications"
          name="notifications"
          checked={settings.notifications}
          onChange={() => handleSettingChange('notifications')}
        />
        
        <Checkbox
          label="Email Updates"
          name="emailUpdates"
          checked={settings.emailUpdates}
          onChange={() => handleSettingChange('emailUpdates')}
        />
        
        <Checkbox
          label="Dark Mode"
          name="darkMode"
          checked={settings.darkMode}
          onChange={() => handleSettingChange('darkMode')}
        />
        
        <Checkbox
          label="Analytics"
          name="analytics"
          checked={settings.analytics}
          onChange={() => handleSettingChange('analytics')}
        />
        
        <Checkbox
          label="Marketing Communications"
          name="marketing"
          checked={settings.marketing}
          onChange={() => handleSettingChange('marketing')}
        />
      </div>

      <div className="settings-summary">
        <p>{Object.values(settings).filter(Boolean).length} of 5 settings enabled</p>
      </div>
    </form>
  );
}
```

**Checkbox Patterns:**

1. **Single checkbox:**
```tsx
const [checked, setChecked] = useState(false);
<input type="checkbox" checked={checked} onChange={(e) => setChecked(e.target.checked)} />
```

2. **Checkbox group:**
```tsx
const [items, setItems] = useState({ item1: false, item2: false });
```

3. **Select all:**
```tsx
const [allSelected, setAllSelected] = useState(false);
```

**Common Errors:**
- Using value instead of checked
- Not handling indeterminate state
- Not grouping related checkboxes
- Not providing clear labels
- Not handling disabled state properly

**Best Practices:**
- Use checked prop, not value
- Provide clear labels
- Group related checkboxes
- Handle indeterminate state
- Use semantic HTML with label elements

---

### 8. Radio

**What is it?**
Radio input for single-choice selection from multiple options, where only one option can be selected at a time.

**Why we need it?**
- Single choice from multiple options
- Mutually exclusive selections
- Rating scales
- Category selection
- Preference choices

**How it works:**
- Multiple inputs with same name
- Only one can be selected at a time
- Value prop determines selection
- onChange updates state
- First option often default

**Syntax:**
```tsx
<input
  type="radio"
  name="groupName"
  value="option1"
  checked={selectedValue === 'option1'}
  onChange={() => setSelectedValue('option1')}
/>
```

**Simple Example:**
```tsx
function GenderSelection() {
  const [gender, setGender] = useState('male');
  
  return (
    <div>
      <label>
        <input
          type="radio"
          name="gender"
          value="male"
          checked={gender === 'male'}
          onChange={() => setGender('male')}
        />
        Male
      </label>
      <label>
        <input
          type="radio"
          name="gender"
          value="female"
          checked={gender === 'female'}
          onChange={() => setGender('female')}
        />
        Female
      </label>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface RadioOption {
  value: string;
  label: string;
  description?: string;
}

interface RadioGroupProps {
  name: string;
  options: RadioOption[];
  selectedValue: string;
  onChange: (value: string) => void;
  disabled?: boolean;
}

function RadioGroup({ name, options, selectedValue, onChange, disabled = false }: RadioGroupProps) {
  return (
    <div className="radio-group">
      {options.map(option => (
        <label key={option.value} className={`radio-option ${disabled ? 'disabled' : ''}`}>
          <input
            type="radio"
            name={name}
            value={option.value}
            checked={selectedValue === option.value}
            onChange={() => onChange(option.value)}
            disabled={disabled}
          />
          <div className="radio-content">
            <span className="radio-label">{option.label}</span>
            {option.description && (
              <span className="radio-description">{option.description}</span>
            )}
          </div>
        </label>
      ))}
    </div>
  );
}

// Usage
function ShippingForm() {
  const [shippingMethod, setShippingMethod] = useState('standard');

  const shippingOptions: RadioOption[] = [
    {
      value: 'standard',
      label: 'Standard Shipping',
      description: '5-7 business days, Free'
    },
    {
      value: 'express',
      label: 'Express Shipping',
      description: '2-3 business days, $9.99'
    },
    {
      value: 'overnight',
      label: 'Overnight Shipping',
      description: 'Next business day, $19.99'
    }
  ];

  return (
    <form className="shipping-form">
      <h3>Select Shipping Method</h3>
      <RadioGroup
        name="shipping"
        options={shippingOptions}
        selectedValue={shippingMethod}
        onChange={setShippingMethod}
      />
      
      <div className="shipping-details">
        <p>Selected: {shippingOptions.find(opt => opt.value === shippingMethod)?.label}</p>
      </div>
    </form>
  );
}
```

**Radio Button Patterns:**

1. **Simple radio group:**
```tsx
const [value, setValue] = useState('option1');
```

2. **With descriptions:**
```tsx
// Include description in option object
```

3. **Conditional options:**
```tsx
// Show/hide based on other selections
```

**Common Errors:**
- Not using same name for group
- Using value instead of checked
- Not providing default selection
- Not handling disabled state
- Poor grouping and labeling

**Best Practices:**
- Use same name for radio groups
- Provide default selection
- Use semantic label elements
- Group related options logically
- Provide helpful descriptions
- Handle disabled state

---

### 9. Select

**What is it?**
Select dropdown for choosing from multiple options, providing a space-efficient way to present many choices.

**Why we need it:**
- Choose from multiple options
- Space-efficient UI
- Standard dropdown behavior
- Keyboard accessible
- Mobile-friendly

**How it works:**
- Controlled component with value prop
- Options array for choices
- onChange handler updates state
- Supports optgroup for grouping
- Can be disabled or read-only

**Syntax:**
```tsx
<select
  value={selectedValue}
  onChange={(e) => setSelectedValue(e.target.value)}
>
  <option value="option1">Option 1</option>
  <option value="option2">Option 2</option>
</select>
```

**Simple Example:**
```tsx
function CountrySelector() {
  const [country, setCountry] = useState('us');
  
  return (
    <div>
      <select value={country} onChange={(e) => setCountry(e.target.value)}>
        <option value="us">United States</option>
        <option value="uk">United Kingdom</option>
        <option value="ca">Canada</option>
      </select>
      <p>Selected: {country}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface SelectOption {
  value: string;
  label: string;
  disabled?: boolean;
}

interface SelectProps {
  label: string;
  name: string;
  options: SelectOption[];
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  disabled?: boolean;
  error?: string;
}

function Select({ label, name, options, value, onChange, placeholder, disabled = false, error }: SelectProps) {
  const handleChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
    onChange(e.target.value);
  };

  return (
    <div className="select-group">
      <label htmlFor={name}>{label}</label>
      <select
        id={name}
        name={name}
        value={value}
        onChange={handleChange}
        disabled={disabled}
        className={error ? 'error' : ''}
      >
        {placeholder && (
          <option value="">{placeholder}</option>
        )}
        {options.map(option => (
          <option
            key={option.value}
            value={option.value}
            disabled={option.disabled}
          >
            {option.label}
          </option>
        ))}
      </select>
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// Usage
function UserProfile() {
  const [role, setRole] = useState('user');
  const [department, setDepartment] = useState('');
  const [experience, setExperience] = useState('');

  const roles: SelectOption[] = [
    { value: 'admin', label: 'Administrator' },
    { value: 'moderator', label: 'Moderator' },
    { value: 'user', label: 'User' },
    { value: 'guest', label: 'Guest' }
  ];

  const departments: SelectOption[] = [
    { value: 'engineering', label: 'Engineering' },
    { value: 'design', label: 'Design' },
    { value: 'marketing', label: 'Marketing' },
    { value: 'sales', label: 'Sales' }
  ];

  const experienceLevels: SelectOption[] = [
    { value: 'junior', label: 'Junior (0-2 years)' },
    { value: 'mid', label: 'Mid-level (3-5 years)' },
    { value: 'senior', label: 'Senior (6-10 years)' },
    { value: 'lead', label: 'Lead (10+ years)' }
  ];

  return (
    <form className="user-profile-form">
      <Select
        label="Role"
        name="role"
        options={roles}
        value={role}
        onChange={setRole}
      />

      <Select
        label="Department"
        name="department"
        options={departments}
        value={department}
        onChange={setDepartment}
        placeholder="Select department"
      />

      <Select
        label="Experience Level"
        name="experience"
        options={experienceLevels}
        value={experience}
        onChange={setExperience}
        placeholder="Select experience level"
      />

      <div className="profile-summary">
        <p>Role: {role}</p>
        <p>Department: {department || 'Not selected'}</p>
        <p>Experience: {experience || 'Not selected'}</p>
      </div>
    </form>
  );
}
```

**Select Features:**
- `multiple`: Allow multiple selections
- `size`: Number of visible options
- `optgroup`: Group related options
- `disabled`: Disable specific options
- `placeholder`: Default option

**Common Errors:**
- Not providing placeholder option
- Not handling disabled options
- Using value instead of onChange
- Not grouping related options
- Poor mobile experience

**Best Practices:**
- Provide placeholder or default value
- Group related options with optgroup
- Handle disabled options properly
- Consider mobile experience
- Use semantic labels

---

### 10. Textarea

**What is it?**
Textarea element for multi-line text input, used for longer content like messages, descriptions, and comments.

**Why we need it?**
- Multi-line text input
- Longer content entry
- Rich text capabilities
- Comments and descriptions
- Better for substantial text

**How it works:**
- Controlled component with value prop
- Supports rows and cols attributes
- onChange handler updates state
- Can have character limits
- Resizable by default

**Syntax:**
```tsx
<textarea
  value={textValue}
  onChange={(e) => setTextValue(e.target.value)}
  rows={4}
  cols={50}
/>
```

**Simple Example:**
```tsx
function MessageInput() {
  const [message, setMessage] = useState('');
  
  return (
    <div>
      <textarea
        value={message}
        onChange={(e) => setMessage(e.target.value)}
        rows={4}
        placeholder="Type your message..."
      />
      <p>Characters: {message.length}</p>
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface TextareaProps {
  label: string;
  name: string;
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  rows?: number;
  cols?: number;
  maxLength?: number;
  disabled?: boolean;
  required?: boolean;
  error?: string;
}

function Textarea({
  label,
  name,
  value,
  onChange,
  placeholder,
  rows = 4,
  cols = 50,
  maxLength,
  disabled = false,
  required = false,
  error
}: TextareaProps) {
  const handleChange = (e: React.ChangeEvent<HTMLTextAreaElement>) => {
    const newValue = e.target.value;
    
    // Enforce maxLength
    if (maxLength && newValue.length > maxLength) {
      return;
    }
    
    onChange(newValue);
  };

  const characterCount = value.length;
  const remainingChars = maxLength ? maxLength - characterCount : null;
  const isNearLimit = maxLength && characterCount > maxLength * 0.9;

  return (
    <div className="textarea-group">
      <label htmlFor={name}>
        {label}
        {required && <span className="required">*</span>}
      </label>
      <textarea
        id={name}
        name={name}
        value={value}
        onChange={handleChange}
        placeholder={placeholder}
        rows={rows}
        cols={cols}
        maxLength={maxLength}
        disabled={disabled}
        required={required}
        className={error ? 'error' : ''}
      />
      {maxLength && (
        <div className="character-count">
          <span className={isNearLimit ? 'warning' : characterCount >= maxLength ? 'error' : ''}>
            {characterCount}
          </span>
          / {maxLength} characters
          {remainingChars !== null && remainingChars > 0 && (
            <span className="remaining">({remainingChars} remaining)</span>
          )}
        </div>
      )}
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}

// Usage
function CommentForm() {
  const [comment, setComment] = useState('');
  const [review, setReview] = useState('');

  return (
    <form className="comment-form">
      <Textarea
        label="Your Comment"
        name="comment"
        value={comment}
        onChange={setComment}
        placeholder="Share your thoughts..."
        rows={4}
        maxLength={500}
        required
      />

      <Textarea
        label="Product Review"
        name="review"
        value={review}
        onChange={setReview}
        placeholder="Write your detailed review..."
        rows={8}
        maxLength={2000}
      />

      <div className="form-actions">
        <button type="submit" disabled={!comment.trim()}>
          Submit Comment
        </button>
      </div>
    </form>
  );
}
```

**Textarea Features:**
- `rows`: Number of visible text lines
- `cols`: Number of visible characters
- `maxLength`: Maximum character count
- `placeholder`: Placeholder text
- `resize`: Control resizing behavior
- `wrap`: Text wrapping behavior

**Common Errors:**
- Not handling character limits
- Not providing appropriate size
- Not handling resize behavior
- Poor mobile experience
- Not providing helpful placeholders

**Best Practices:**
- Set appropriate rows and cols
- Implement character limits
- Provide helpful placeholders
- Consider mobile responsiveness
- Handle resize behavior
- Show character count when limited

---

### 11. Multiple Inputs

**What is it?**
Managing multiple form inputs in a single component, organizing state and handlers efficiently.

**Why we need it:**
- Most forms have multiple fields
- Need efficient state management
- Consistent handling patterns
- Better code organization
- Easier maintenance

**How it works:**
- Object state for multiple fields
- Single handler for all inputs
- Name attribute identifies field
- Update pattern for efficiency
- TypeScript interfaces for type safety

**Syntax:**
```tsx
const [formData, setFormData] = useState({
  field1: '',
  field2: '',
  field3: ''
});

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setFormData(prev => ({ ...prev, [name]: value }));
};
```

**Simple Example:**
```tsx
function RegistrationForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: ''
  });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form>
      <input name="username" value={formData.username} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <input name="password" value={formData.password} onChange={handleChange} />
    </form>
  );
}
```

**Real-world Example:**
```tsx
interface FormData {
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
  address: string;
  city: string;
  state: string;
  zipCode: string;
  country: string;
}

function ShippingForm() {
  const [formData, setFormData] = useState<FormData>({
    firstName: '',
    lastName: '',
    email: '',
    phone: '',
    address: '',
    city: '',
    state: '',
    zipCode: '',
    country: ''
  });

  const [touched, setTouched] = useState<Record<keyof FormData, boolean>>({
    firstName: false,
    lastName: false,
    email: false,
    phone: false,
    address: false,
    city: false,
    state: false,
    zipCode: false,
    country: false
  });

  const [errors, setErrors] = useState<Partial<Record<keyof FormData, string>>>({});

  const validateField = (field: keyof FormData, value: string): string | undefined => {
    switch (field) {
      case 'firstName':
      case 'lastName':
        return value.trim() ? undefined : 'This field is required';
      case 'email':
        return !/\S+@\S+\.\S+/.test(value) ? 'Invalid email address' : undefined;
      case 'phone':
        return !/^\d{10}$/.test(value.replace(/\D/g, '')) ? 'Invalid phone number' : undefined;
      case 'zipCode':
        return !/^\d{5}(-\d{4})?$/.test(value) ? 'Invalid ZIP code' : undefined;
      default:
        return value.trim() ? undefined : 'This field is required';
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Validate on change if field has been touched
    if (touched[name]) {
      const error = validateField(name as keyof FormData, value);
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name as keyof FormData, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    // Validate all fields
    const newErrors: Partial<Record<keyof FormData, string>> = {};
    let isValid = true;

    (Object.keys(formData) as Array<keyof FormData>).forEach(field => {
      const error = validateField(field, formData[field]);
      if (error) {
        newErrors[field] = error;
        isValid = false;
      }
    });

    setErrors(newErrors);
    setTouched({
      firstName: true,
      lastName: true,
      email: true,
      phone: true,
      address: true,
      city: true,
      state: true,
      zipCode: true,
      country: true
    });

    if (isValid) {
      console.log('Form submitted:', formData);
      alert('Form submitted successfully!');
    }
  };

  const resetForm = () => {
    setFormData({
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
      address: '',
      city: '',
      state: '',
      zipCode: '',
      country: ''
    });
    setErrors({});
    setTouched({
      firstName: false,
      lastName: false,
      email: false,
      phone: false,
      address: false,
      city: false,
      state: false,
      zipCode: false,
      country: false
    });
  };

  const isFormValid = Object.keys(errors).length === 0 &&
    Object.values(touched).every(Boolean) &&
    Object.values(formData).every(value => value.trim().length > 0);

  return (
    <form onSubmit={handleSubmit} className="shipping-form">
      <h2>Shipping Information</h2>

      <div className="form-row">
        <div className="form-group">
          <label>First Name</label>
          <input
            type="text"
            name="firstName"
            value={formData.firstName}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.firstName ? 'error' : ''}
          />
          {errors.firstName && <span className="error-message">{errors.firstName}</span>}
        </div>

        <div className="form-group">
          <label>Last Name</label>
          <input
            type="text"
            name="lastName"
            value={formData.lastName}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.lastName ? 'error' : ''}
          />
          {errors.lastName && <span className="error-message">{errors.lastName}</span>}
        </div>
      </div>

      <div className="form-group">
        <label>Email</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>

      <div className="form-group">
        <label>Phone</label>
        <input
          type="tel"
          name="phone"
          value={formData.phone}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.phone ? 'error' : ''}
        />
        {errors.phone && <span className="error-message">{errors.phone}</span>}
      </div>

      <div className="form-group">
        <label>Address</label>
        <input
          type="text"
          name="address"
          value={formData.address}
          onChange={handleChange}
          onBlur={handleBlur}
          className={errors.address ? 'error' : ''}
        />
        {errors.address && <span className="error-message">{errors.address}</span>}
      </div>

      <div className="form-row">
        <div className="form-group">
          <label>City</label>
          <input
            type="text"
            name="city"
            value={formData.city}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.city ? 'error' : ''}
          />
          {errors.city && <span className="error-message">{errors.city}</span>}
        </div>

        <div className="form-group">
          <label>State</label>
          <input
            type="text"
            name="state"
            value={formData.state}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.state ? 'error' : ''}
          />
          {errors.state && <span className="error-message">{errors.state}</span>}
        </div>
      </div>

      <div className="form-row">
        <div className="form-group">
          <label>ZIP Code</label>
          <input
            type="text"
            name="zipCode"
            value={formData.zipCode}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.zipCode ? 'error' : ''}
          />
          {errors.zipCode && <span className="error-message">{errors.zipCode}</span>}
        </div>

        <div className="form-group">
          <label>Country</label>
          <input
            type="text"
            name="country"
            value={formData.country}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.country ? 'error' : ''}
          />
          {errors.country && <span className="error-message">{errors.country}</span>}
        </div>
      </div>

      <div className="form-actions">
        <button type="button" onClick={resetForm}>Reset</button>
        <button type="submit" disabled={!isFormValid}>Submit</button>
      </div>
    </form>
  );
}
```

**Multiple Input Patterns:**

1. **Object state with single handler:**
```tsx
const [formData, setFormData] = useState({ field1: '', field2: '' });
const handleChange = (e) => {
  const { name, value } = e.target;
  setFormData(prev => ({ ...prev, [name]: value }));
};
```

2. **Multiple state variables:**
```tsx
const [field1, setField1] = useState('');
const [field2, setField2] = useState('');
```

3. **Nested object state:**
```tsx
const [formData, setFormData] = useState({
  user: { name: '', email: '' },
  address: { street: '', city: '' }
});
```

**Common Errors:**
- Not using name attributes
- Complex state when simple would work
- Not handling validation properly
- Not organizing related fields
- Inconsistent naming conventions

**Best Practices:**
- Use object state for related fields
- Single handler for efficiency
- TypeScript interfaces for type safety
- Organize fields logically
- Consistent naming conventions

---

### 12. Form submit

**What is it?**
The process of handling form submission in React, including preventing default browser behavior and processing form data.

**Why we need it:**
- Prevent page reloads
- Process form data in JavaScript
- Implement validation before submission
- Handle async operations
- Provide user feedback

**How it works:**
- onSubmit event handler
- Prevent default behavior
- Access form data from state
- Validate before processing
- Handle success/error states

**Syntax:**
```tsx
function MyForm() {
  const [formData, setFormData] = useState({ field: '' });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* form fields */}
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Simple Example:**
```tsx
function SimpleForm() {
  const [name, setName] = useState('');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    alert(`Hello, ${name}!`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Your name"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Real-world Example:**
```tsx
interface FormData {
  username: string;
  email: string;
  age: number;
}

function UserProfileForm() {
  const [formData, setFormData] = useState<FormData>({
    username: '',
    email: '',
    age: 0
  });

  const [errors, setErrors] = useState<Partial<Record<keyof FormData, string>>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitSuccess, setSubmitSuccess] = useState(false);
  const [submitError, setSubmitError] = useState<string | null>(null);

  const validateForm = (): boolean => {
    const newErrors: Partial<Record<keyof FormData, string>> = {};

    if (!formData.username.trim()) {
      newErrors.username = 'Username is required';
    } else if (formData.username.length < 3) {
      newErrors.username = 'Username must be at least 3 characters';
    }

    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Invalid email format';
    }

    if (formData.age < 18 || formData.age > 120) {
      newErrors.age = 'Age must be between 18 and 120';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) {
      return;
    }

    setIsSubmitting(true);
    setSubmitError(null);
    setSubmitSuccess(false);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));

      console.log('Form submitted:', formData);
      setSubmitSuccess(true);

      // Reset form after successful submission
      setTimeout(() => {
        setFormData({ username: '', email: '', age: 0 });
        setSubmitSuccess(false);
      }, 3000);

    } catch (error) {
      setSubmitError('Submission failed. Please try again.');
      console.error('Submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const resetForm = () => {
    setFormData({ username: '', email: '', age: 0 });
    setErrors({});
    setSubmitSuccess(false);
    setSubmitError(null);
  };

  return (
    <div className="user-profile-form">
      <h2>User Profile</h2>

      {submitSuccess && (
        <div className="success-message">
          <h3>Profile Updated Successfully!</h3>
          <p>Your changes have been saved.</p>
        </div>
      )}

      {submitError && (
        <div className="error-message">
          <h3>Error</h3>
          <p>{submitError}</p>
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label htmlFor="username">Username</label>
          <input
            type="text"
            id="username"
            name="username"
            value={formData.username}
            onChange={(e) => setFormData({ ...formData, username: e.target.value })}
            className={errors.username ? 'error' : ''}
          />
          {errors.username && <span className="error-text">{errors.username}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={(e) => setFormData({ ...formData, email: e.target.value })}
            className={errors.email ? 'error' : ''}
          />
          {errors.email && <span className="error-text">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="age">Age</label>
          <input
            type="number"
            id="age"
            name="age"
            value={formData.age}
            onChange={(e) => setFormData({ ...formData, age: Number(e.target.value) })}
            min={18}
            max={120}
            className={errors.age ? 'error' : ''}
          />
          {errors.age && <span className="error-text">{errors.age}</span>}
        </div>

        <div className="form-actions">
          <button type="button" onClick={resetForm} disabled={isSubmitting}>
            Reset
          </button>
          <button type="submit" disabled={isSubmitting}>
            {isSubmitting ? 'Submitting...' : 'Save Profile'}
          </button>
        </div>
      </form>
    </div>
  );
}
```

**Form Submission Patterns:**

1. **Simple submission:**
```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  console.log(formData);
};
```

2. **Async submission:**
```tsx
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  setIsLoading(true);
  await api.submit(formData);
  setIsLoading(false);
};
```

3. **With validation:**
```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  if (validateForm()) {
    processFormData();
  }
};
```

**Common Errors:**
- Not preventing default behavior
- Not handling async operations
- Not providing loading feedback
- Not handling errors properly
- Not validating before submission

**Best Practices:**
- Always prevent default in React forms
- Handle async operations properly
- Provide loading feedback
- Implement proper validation
- Handle errors gracefully
- Reset form after successful submission

---

### 13. preventDefault

**What is it?**
The preventDefault() method called on React synthetic events to stop the browser's default behavior for form submissions and other events.

**Why we need it?**
- Prevent page reload on form submit
- Stop default link navigation
- Prevent default browser behavior
- Implement custom form handling
- Better user experience

**How it works:**
- Called on synthetic event object
- Stops browser's default action
- Allows custom JavaScript handling
- Works with various event types
- Essential for React forms

**Syntax:**
```tsx
const handleSubmit = (e: React.FormEvent) => {
  e.preventDefault();
  // Custom handling
};
```

**Simple Example:**
```tsx
function LinkForm() {
  const handleClick = (e: React.MouseEvent) => {
    e.preventDefault();
    console.log('Link clicked, but navigation prevented');
  };

  return (
    <a href="https://example.com" onClick={handleClick}>
      Click me (won't navigate)
    </a>
  );
}
```

**Real-world Example:**
```tsx
function CustomFormHandling() {
  const [formData, setFormData] = useState({
    searchQuery: '',
    category: 'all',
    sortBy: 'relevance'
  });

  const handleSearchSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log('Search with filters:', formData);
    // Custom search logic instead of form submission
  };

  const handleLinkClick = (e: React.MouseEvent, url: string) => {
    e.preventDefault();
    // Custom navigation logic
    console.log('Navigating to:', url);
    window.history.pushState({}, '', url);
  };

  const handleContextMenu = (e: React.MouseEvent) => {
    e.preventDefault();
    // Custom context menu
    console.log('Custom context menu at:', { x: e.clientX, y: e.clientY });
  };

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === ' ' && e.target instanceof HTMLBodyElement) {
      e.preventDefault(); // Prevent page scroll with space
      console.log('Space pressed, default prevented');
    }
  };

  return (
    <div className="custom-form-handling">
      <h2>Custom Form Handling Examples</h2>

      {/* Form submission */}
      <form onSubmit={handleSearchSubmit}>
        <input
          type="text"
          value={formData.searchQuery}
          onChange={(e) => setFormData({ ...formData, searchQuery: e.target.value })}
          placeholder="Search..."
        />
        <select
          value={formData.category}
          onChange={(e) => setFormData({ ...formData, category: e.target.value })}
        >
          <option value="all">All Categories</option>
          <option value="electronics">Electronics</option>
          <option value="clothing">Clothing</option>
        </select>
        <button type="submit">Search</button>
      </form>

      {/* Link handling */}
      <div className="link-handling">
        <h3>Custom Link Handling</h3>
        <a href="/page1" onClick={(e) => handleLinkClick(e, '/page1')}>
          Custom Navigation to Page 1
        </a>
        <a href="/page2" onClick={(e) => handleLinkClick(e, '/page2')}>
          Custom Navigation to Page 2
        </a>
      </div>

      {/* Context menu */}
      <div 
        className="context-area"
        onContextMenu={handleContextMenu}
      >
        <h3>Right-click here for custom context menu</h3>
      </div>

      {/* Keyboard events */}
      <div onKeyDown={handleKeyDown} tabIndex={0} className="keyboard-area">
        <h3>Press space (default scrolling prevented)</h3>
      </div>
    </div>
  );
}
```

**Common preventDefault Use Cases:**

1. **Form submission:**
```tsx
e.preventDefault(); // Prevent page reload
```

2. **Link clicks:**
```tsx
e.preventDefault(); // Prevent navigation
```

3. **Form validation:**
```tsx
e.preventDefault(); // Stop invalid submission
```

4. **Keyboard shortcuts:**
```tsx
e.preventDefault(); // Prevent default key behavior
```

5. **Drag and drop:**
```tsx
e.preventDefault(); // Prevent default drag behavior
```

**Common Errors:**
- Forgetting to call preventDefault
- Calling it on wrong events
- Not understanding when it's needed
- Preventing too many defaults
- Not testing default behavior

**Best Practices:**
- Always prevent default in React forms
- Understand what you're preventing
- Test that prevention is necessary
- Document why prevention is needed
- Consider accessibility implications

---

### 14. Form state

**What is it?**
The React state that holds all form data, providing a single source of truth for form values and enabling controlled components.

**Why we need it:**
- Single source of truth for form data
- Enables validation and feedback
- Controls form behavior
- Simplifies form handling
- Better user experience

**How it works:**
- State object with form field values
- Updated via onChange handlers
- Re-renders when state changes
- Submitted when form is valid
- Can be reset easily

**Syntax:**
```tsx
const [formData, setFormData] = useState({
  field1: '',
  field2: '',
  field3: ''
});
```

**Simple Example:**
```tsx
function SimpleFormState() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    age: ''
  });

  return (
    <form>
      <input
        value={formData.username}
        onChange={(e) => setFormData({ ...formData, username: e.target.value })}
      />
      <input
        value={formData.email}
        onChange={(e) => setFormData({ ...formData, email: e.target.value })}
      />
      <input
        value={formData.age}
        onChange={(e) => setFormData({ ...formData, age: e.target.value })}
      />
    </form>
  );
}
```

**Real-world Example:**
```tsx
interface ComplexFormState {
  personalInfo: {
    firstName: string;
    lastName: string;
    email: string;
    phone: string;
  };
  address: {
    street: string;
    city: string;
    state: string;
    zipCode: string;
    country: string;
  };
  preferences: {
    emailNotifications: boolean;
    smsNotifications: boolean;
    language: string;
    timezone: string;
  };
  security: {
    password: string;
    confirmPassword: string;
    twoFactorEnabled: boolean;
  };
}

function ComprehensiveForm() {
  const [formData, setFormData] = useState<ComplexFormState>({
    personalInfo: {
      firstName: '',
      lastName: '',
      email: '',
      phone: ''
    },
    address: {
      street: '',
      city: '',
      state: '',
      zipCode: '',
      country: ''
    },
    preferences: {
      emailNotifications: true,
      smsNotifications: false,
      language: 'en',
      timezone: 'UTC'
    },
    security: {
      password: '',
      confirmPassword: '',
      twoFactorEnabled: false
    }
  });

  const [activeSection, setActiveSection] = useState<'personal' | 'address' | 'preferences' | 'security'>('personal');
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitSuccess, setSubmitSuccess] = useState(false);

  const updatePersonalInfo = (field: keyof ComplexFormState['personalInfo'], value: string) => {
    setFormData(prev => ({
      ...prev,
      personalInfo: { ...prev.personalInfo, [field]: value }
    }));
  };

  const updateAddress = (field: keyof ComplexFormState['address'], value: string) => {
    setFormData(prev => ({
      ...prev,
      address: { ...prev.address, [field]: value }
    }));
  };

  const updatePreferences = (field: keyof ComplexFormState['preferences'], value: any) => {
    setFormData(prev => ({
      ...prev,
      preferences: { ...prev.preferences, [field]: value }
    }));
  };

  const updateSecurity = (field: keyof ComplexFormState['security'], value: any) => {
    setFormData(prev => ({
      ...prev,
      security: { ...prev.security, [field]: value }
    }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsSubmitting(true);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Form submitted:', formData);
      setSubmitSuccess(true);
    } catch (error) {
      console.error('Submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const resetForm = () => {
    setFormData({
      personalInfo: { firstName: '', lastName: '', email: '', phone: '' },
      address: { street: '', city: '', state: '', zipCode: '', country: '' },
      preferences: { emailNotifications: true, smsNotifications: false, language: 'en', timezone: 'UTC' },
      security: { password: '', confirmPassword: '', twoFactorEnabled: false }
    });
    setSubmitSuccess(false);
  };

  return (
    <div className="comprehensive-form">
      <h2>Comprehensive Form</h2>

      {/* Section Navigation */}
      <div className="section-nav">
        <button
          className={activeSection === 'personal' ? 'active' : ''}
          onClick={() => setActiveSection('personal')}
        >
          Personal Info
        </button>
        <button
          className={activeSection === 'address' ? 'active' : ''}
          onClick={() => setActiveSection('address')}
        >
          Address
        </button>
        <button
          className={activeSection === 'preferences' ? 'active' : ''}
          onClick={() => setActiveSection('preferences')}
        >
          Preferences
        </button>
        <button
          className={activeSection === 'security' ? 'active' : ''}
          onClick={() => setActiveSection('security')}
        >
          Security
        </button>
      </div>

      {submitSuccess && (
        <div className="success-banner">
          <h3>Form submitted successfully!</h3>
          <button onClick={() => setSubmitSuccess(false)}>Dismiss</button>
        </div>
      )}

      <form onSubmit={handleSubmit}>
        {/* Personal Info Section */}
        {activeSection === 'personal' && (
          <fieldset>
            <legend>Personal Information</legend>
            <div className="form-row">
              <div className="form-group">
                <label>First Name</label>
                <input
                  type="text"
                  value={formData.personalInfo.firstName}
                  onChange={(e) => updatePersonalInfo('firstName', e.target.value)}
                />
              </div>
              <div className="form-group">
                <label>Last Name</label>
                <input
                  type="text"
                  value={formData.personalInfo.lastName}
                  onChange={(e) => updatePersonalInfo('lastName', e.target.value)}
                />
              </div>
            </div>
            <div className="form-group">
              <label>Email</label>
              <input
                type="email"
                value={formData.personalInfo.email}
                onChange={(e) => updatePersonalInfo('email', e.target.value)}
              />
            </div>
            <div className="form-group">
              <label>Phone</label>
              <input
                type="tel"
                value={formData.personalInfo.phone}
                onChange={(e) => updatePersonalInfo('phone', e.target.value)}
              />
            </div>
          </fieldset>
        )}

        {/* Address Section */}
        {activeSection === 'address' && (
          <fieldset>
            <legend>Address Information</legend>
            <div className="form-group">
              <label>Street Address</label>
              <input
                type="text"
                value={formData.address.street}
                onChange={(e) => updateAddress('street', e.target.value)}
              />
            </div>
            <div className="form-row">
              <div className="form-group">
                <label>City</label>
                <input
                  type="text"
                  value={formData.address.city}
                  onChange={(e) => updateAddress('city', e.target.value)}
                />
              </div>
              <div className="form-group">
                <label>State</label>
                <input
                  type="text"
                  value={formData.address.state}
                  onChange={(e) => updateAddress('state', e.target.value)}
                />
              </div>
            </div>
            <div className="form-row">
              <div className="form-group">
                <label>ZIP Code</label>
                <input
                  type="text"
                  value={formData.address.zipCode}
                  onChange={(e) => updateAddress('zipCode', e.target.value)}
                />
              </div>
              <div className="form-group">
                <label>Country</label>
                <input
                  type="text"
                  value={formData.address.country}
                  onChange={(e) => updateAddress('country', e.target.value)}
                />
              </div>
            </div>
          </fieldset>
        )}

        {/* Preferences Section */}
        {activeSection === 'preferences' && (
          <fieldset>
            <legend>Preferences</legend>
            <div className="form-group">
              <label>
                <input
                  type="checkbox"
                  checked={formData.preferences.emailNotifications}
                  onChange={(e) => updatePreferences('emailNotifications', e.target.checked)}
                />
                Email Notifications
              </label>
            </div>
            <div className="form-group">
              <label>
                <input
                  type="checkbox"
                  checked={formData.preferences.smsNotifications}
                  onChange={(e) => updatePreferences('smsNotifications', e.target.checked)}
                />
                SMS Notifications
              </label>
            </div>
            <div className="form-group">
              <label>Language</label>
              <select
                value={formData.preferences.language}
                onChange={(e) => updatePreferences('language', e.target.value)}
              >
                <option value="en">English</option>
                <option value="es">Spanish</option>
                <option value="fr">French</option>
                <option value="de">German</option>
              </select>
            </div>
            <div className="form-group">
              <label>Timezone</label>
              <select
                value={formData.preferences.timezone}
                onChange={(e) => updatePreferences('timezone', e.target.value)}
              >
                <option value="UTC">UTC</option>
                <option value="EST">Eastern Time</option>
                <option value="PST">Pacific Time</option>
                <option value="CET">Central European Time</option>
              </select>
            </div>
          </fieldset>
        )}

        {/* Security Section */}
        {activeSection === 'security' && (
          <fieldset>
            <legend>Security Settings</legend>
            <div className="form-group">
              <label>Password</label>
              <input
                type="password"
                value={formData.security.password}
                onChange={(e) => updateSecurity('password', e.target.value)}
              />
            </div>
            <div className="form-group">
              <label>Confirm Password</label>
              <input
                type="password"
                value={formData.security.confirmPassword}
                onChange={(e) => updateSecurity('confirmPassword', e.target.value)}
              />
            </div>
            <div className="form-group">
              <label>
                <input
                  type="checkbox"
                  checked={formData.security.twoFactorEnabled}
                  onChange={(e) => updateSecurity('twoFactorEnabled', e.target.checked)}
                />
                Enable Two-Factor Authentication
              </label>
            </div>
          </fieldset>
        )}

        <div className="form-actions">
          <button type="button" onClick={resetForm}>Reset Form</button>
          <button type="submit" disabled={isSubmitting}>
            {isSubmitting ? 'Submitting...' : 'Submit'}
          </button>
        </div>
      </form>
    </div>
  );
}
```

**Form State Patterns:**

1. **Flat object:**
```tsx
const [formData, setFormData] = useState({
  field1: '',
  field2: '',
  field3: ''
});
```

2. **Nested object:**
```tsx
const [formData, setFormData] = useState({
  section1: { field1: '', field2: '' },
  section2: { field3: '', field4: '' }
});
```

3. **With validation state:**
```tsx
const [formData, setFormData] = useState(initialData);
const [errors, setErrors] = useState({});
const [touched, setTouched] = useState({});
```

**Common Errors:**
- Overly complex state structures
- Not organizing related fields
- Mixing unrelated data
- Not handling nested updates properly
- State duplication

**Best Practices:**
- Organize related fields
- Use TypeScript interfaces
- Keep state structure manageable
- Consider splitting large forms
- Validate state properly

---

### 15. Validation

**What is it?**
The process of checking form data against rules and constraints to ensure data integrity and user experience.

**Why we need it:**
- Ensure data quality
- Provide user feedback
- Prevent invalid submissions
- Improve user experience
- Protect backend from bad data

**How it works:**
- Define validation rules
- Check form data against rules
- Show error messages
- Prevent invalid submissions
- Can be real-time or on submit

**Syntax:**
```tsx
const validateField = (value: string): string | undefined => {
  if (!value.trim()) return 'This field is required';
  if (value.length < 3) return 'Minimum 3 characters';
  return undefined;
};
```

**Simple Example:**
```tsx
function ValidatedInput() {
  const [value, setValue] = useState('');
  const [error, setError] = useState('');

  const validate = (input: string): string => {
    if (!input.trim()) return 'This field is required';
    if (input.length < 3) return 'Minimum 3 characters';
    return '';
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newValue = e.target.value;
    setValue(newValue);
    setError(validate(newValue));
  };

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        className={error ? 'error' : ''}
      />
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface ValidationRule {
  required?: boolean;
  minLength?: number;
  maxLength?: number;
  pattern?: RegExp;
  custom?: (value: string) => string | undefined;
}

interface ValidationState {
  [fieldName: string]: {
    error: string | null;
    touched: boolean;
    isValid: boolean;
  };
}

function ComprehensiveValidationForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    confirmPassword: '',
    age: ''
  });

  const [validation, setValidation] = useState<ValidationState>({});

  const validationRules: Record<string, ValidationRule> = {
    username: {
      required: true,
      minLength: 3,
      maxLength: 20,
      pattern: /^[a-zA-Z0-9_]+$/,
      custom: (value) => {
        const commonUsernames = ['admin', 'user', 'test'];
        if (commonUsernames.includes(value.toLowerCase())) {
          return 'Username is not available';
        }
        return undefined;
      }
    },
    email: {
      required: true,
      pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
      custom: (value) => {
        const disposableDomains = ['tempmail.com', 'throwaway.com'];
        const domain = value.split('@')[1];
        if (disposableDomains.includes(domain)) {
          return 'Disposable emails are not allowed';
        }
        return undefined;
      }
    },
    password: {
      required: true,
      minLength: 8,
      custom: (value) => {
        if (!/[A-Z]/.test(value)) return 'Must contain uppercase letter';
        if (!/[a-z]/.test(value)) return 'Must contain lowercase letter';
        if (!/[0-9]/.test(value)) return 'Must contain number';
        if (!/[^A-Za-z0-9]/.test(value)) return 'Must contain special character';
        return undefined;
      }
    },
    confirmPassword: {
      required: true,
      custom: (value, allValues) => {
        if (value !== allValues.password) return 'Passwords do not match';
        return undefined;
      }
    },
    age: {
      required: true,
      custom: (value) => {
        const age = Number(value);
        if (isNaN(age)) return 'Must be a number';
        if (age < 18) return 'Must be at least 18';
        if (age > 120) return 'Must be less than 120';
        return undefined;
      }
    }
  };

  const validateField = (fieldName: string, value: string): string | null => {
    const rules = validationRules[fieldName];
    if (!rules) return null;

    if (rules.required && !value.trim()) {
      return 'This field is required';
    }

    if (rules.minLength && value.length < rules.minLength) {
      return `Minimum ${rules.minLength} characters required`;
    }

    if (rules.maxLength && value.length > rules.maxLength) {
      return `Maximum ${rules.maxLength} characters allowed`;
    }

    if (rules.pattern && !rules.pattern.test(value)) {
      return 'Invalid format';
    }

    if (rules.custom) {
      return rules.custom(value, formData);
    }

    return null;
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));

    // Validate on change if field has been touched
    if (validation[name]?.touched) {
      const error = validateField(name, value);
      setValidation(prev => ({
        ...prev,
        [name]: {
          ...prev[name],
          error,
          isValid: !error,
          touched: true
        }
      }));
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    const error = validateField(name, value);

    setValidation(prev => ({
      ...prev,
      [name]: {
        error,
        isValid: !error,
        touched: true
      }
    }));
  };

  const validateForm = (): boolean => {
    let isValid = true;
    const newValidation: ValidationState = {};

    Object.keys(formData).forEach(fieldName => {
      const error = validateField(fieldName, formData[fieldName as keyof typeof formData]);
      newValidation[fieldName] = {
        error,
        isValid: !error,
        touched: true
      };
      if (error) isValid = false;
    });

    setValidation(newValidation);
    return isValid;
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    if (validateForm()) {
      console.log('Form is valid:', formData);
      alert('Form submitted successfully!');
    } else {
      console.log('Form has errors');
    }
  };

  const isFormValid = Object.values(validation).every(field => field.isValid);

  return (
    <div className="validation-form">
      <h2>Form with Comprehensive Validation</h2>

      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label>Username</label>
          <input
            type="text"
            name="username"
            value={formData.username}
            onChange={handleChange}
            onBlur={handleBlur}
            className={validation.username?.error ? 'error' : ''}
          />
          {validation.username?.error && (
            <span className="error-message">{validation.username.error}</span>
          )}
          {validation.username?.touched && !validation.username.error && (
            <span className="success-message">✓ Available</span>
          )}
        </div>

        <div className="form-group">
          <label>Email</label>
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            onBlur={handleBlur}
            className={validation.email?.error ? 'error' : ''}
          />
          {validation.email?.error && (
            <span className="error-message">{validation.email.error}</span>
          )}
        </div>

        <div className="form-group">
          <label>Password</label>
          <input
            type="password"
            name="password"
            value={formData.password}
            onChange={handleChange}
            onBlur={handleBlur}
            className={validation.password?.error ? 'error' : ''}
          />
          {validation.password?.error && (
            <span className="error-message">{validation.password.error}</span>
          )}
        </div>

        <div className="form-group">
          <label>Confirm Password</label>
          <input
            type="password"
            name="confirmPassword"
            value={formData.confirmPassword}
            onChange={handleChange}
            onBlur={handleBlur}
            className={validation.confirmPassword?.error ? 'error' : ''}
          />
          {validation.confirmPassword?.error && (
            <span className="error-message">{validation.confirmPassword.error}</span>
          )}
        </div>

        <div className="form-group">
          <label>Age</label>
          <input
            type="number"
            name="age"
            value={formData.age}
            onChange={handleChange}
            onBlur={handleBlur}
            className={validation.age?.error ? 'error' : ''}
          />
          {validation.age?.error && (
            <span className="error-message">{validation.age.error}</span>
          )}
        </div>

        <button type="submit" disabled={!isFormValid}>
          Submit
        </button>
      </form>
    </div>
  );
}
```

**Validation Patterns:**

1. **Real-time validation:**
```tsx
const handleChange = (e) => {
  setValue(e.target.value);
  setError(validate(e.target.value));
};
```

2. **On-blur validation:**
```tsx
const handleBlur = (e) => {
  setError(validate(e.target.value));
};
```

3. **On-submit validation:**
```tsx
const handleSubmit = (e) => {
  e.preventDefault();
  if (validateForm()) {
    submit();
  }
};
```

**Common Errors:**
- Not providing clear error messages
- Validating at wrong times
- Not handling all edge cases
- Overly complex validation rules
- Not providing success feedback

**Best Practices:**
- Provide clear, specific error messages
- Validate at appropriate times
- Handle all edge cases
- Keep validation rules simple
- Provide visual feedback for valid inputs

---

### 16. Error messages

**What is it?**
User-facing messages that inform users about validation errors, form submission problems, or other issues with their input.

**Why we need it?**
- Guide users to correct mistakes
- Improve user experience
- Provide clear feedback
- Prevent frustration
- Ensure data quality

**How it works:**
- Stored in validation state
- Displayed near relevant fields
- Updated as user corrects errors
- Can be inline or summary
- Should be clear and actionable

**Syntax:**
```tsx
{error && <span className="error-message">{error}</span>}
```

**Simple Example:**
```tsx
function InputWithError() {
  const [value, setValue] = useState('');
  const [error, setError] = useState('');

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const newValue = e.target.value;
    setValue(newValue);
    if (newValue.length < 3) {
      setError('Minimum 3 characters required');
    } else {
      setError('');
    }
  };

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        className={error ? 'error' : ''}
      />
      {error && <span className="error-message">{error}</span>}
    </div>
  );
}
```

**Real-world Example:**
```tsx
interface FormFieldProps {
  label: string;
  name: string;
  value: string;
  onChange: (value: string) => void;
  error?: string;
  helperText?: string;
  required?: boolean;
}

function FormField({ label, name, value, onChange, error, helperText, required = false }: FormFieldProps) {
  const hasError = !!error;
  const hasHelper = !!helperText;

  return (
    <div className={`form-field ${hasError ? 'has-error' : ''}`}>
      <label htmlFor={name}>
        {label}
        {required && <span className="required-indicator">*</span>}
      </label>
      <input
        id={name}
        name={name}
        value={value}
        onChange={(e) => onChange(e.target.value)}
        className={hasError ? 'error' : ''}
        aria-invalid={hasError}
        aria-describedby={hasError ? `${name}-error` : helperText ? `${name}-helper` : undefined}
      />
      
      {hasHelper && (
        <span id={`${name}-helper`} className="helper-text">
          {helperText}
        </span>
      )}
      
      {hasError && (
        <span id={`${name}-error`} className="error-message" role="alert">
          <span className="error-icon">⚠️</span>
          {error}
        </span>
      )}
    </div>
  );
}

function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    subject: '',
    message: ''
  });

  const [errors, setErrors] = useState<Record<string, string>>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});

  const validateField = (name: string, value: string): string | undefined => {
    switch (name) {
      case 'name':
        return value.trim().length < 2 ? 'Name must be at least 2 characters' : undefined;
      case 'email':
        return !/\S+@\S+\.\S+/.test(value) ? 'Please enter a valid email' : undefined;
      case 'subject':
        return value.trim().length < 5 ? 'Subject must be at least 5 characters' : undefined;
      case 'message':
        return value.trim().length < 10 ? 'Message must be at least 10 characters' : undefined;
      default:
        return undefined;
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Validate on change if field has been touched
    if (touched[name]) {
      const error = validateField(name, value);
      setErrors(prev => ({ ...prev, [name]: error || '' }));
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name, value);
    setErrors(prev => ({ ...prev, [name]: error || '' }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    // Validate all fields
    const newErrors: Record<string, string> = {};
    let isValid = true;

    Object.keys(formData).forEach(field => {
      const error = validateField(field, formData[field as keyof typeof formData]);
      if (error) {
        newErrors[field] = error;
        isValid = false;
      }
    });

    setErrors(newErrors);
    setTouched({
      name: true,
      email: true,
      subject: true,
      message: true
    });

    if (isValid) {
      console.log('Form submitted:', formData);
      alert('Message sent successfully!');
    }
  };

  const isFormValid = Object.keys(errors).length === 0;

  return (
    <div className="contact-form">
      <h2>Contact Us</h2>

      <div className="error-summary">
        {Object.keys(errors).length > 0 && (
          <div className="summary" role="alert">
            <h3>Please fix the following errors:</h3>
            <ul>
              {Object.entries(errors).map(([field, error]) => (
                <li key={field}>{error}</li>
              ))}
            </ul>
          </div>
        )}
      </div>

      <form onSubmit={handleSubmit}>
        <FormField
          label="Name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          onBlur={handleBlur}
          error={errors.name}
          helperText="Enter your full name"
          required
        />

        <FormField
          label="Email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
          error={errors.email}
          helperText="We'll never share your email"
          required
        />

        <FormField
          label="Subject"
          name="subject"
          value={formData.subject}
          onChange={handleChange}
          onBlur={handleBlur}
          error={errors.subject}
          helperText="Brief description of your inquiry"
          required
        />

        <FormField
          label="Message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          onBlur={handleBlur}
          error={errors.message}
          helperText="Provide details about your inquiry"
          required
        >
          <textarea rows={5} />
        </FormField>

        <button type="submit" disabled={!isFormValid}>
          Send Message
        </button>
      </form>
    </div>
  );
}
```

**Error Message Patterns:**

1. **Inline errors:**
```tsx
{error && <span className="error">{error}</span>}
```

2. **Field-level errors:**
```tsx
<div className="field-error">{error}</div>
```

3. **Form-level summary:**
```tsx
<div className="error-summary">
  <h3>Please fix these errors:</h3>
  <ul>{errors.map(err => <li>{err}</li>)}</ul>
</div>
```

**Common Errors:**
- Unclear error messages
- Technical jargon
- Not providing solutions
- Too many errors at once
- Not removing errors when corrected

**Best Practices:**
- Be specific and actionable
- Use plain language
- Provide solutions
- Show errors at right time
- Remove errors when corrected
- Use accessibility attributes

---

### 17. Reset form

**What is it?**
The process of resetting form state back to initial values, clearing user input and validation errors.

**Why we need it?**
- Clear form after submission
- Allow users to start over
- Reset validation state
- Provide clear form state
- Better user experience

**How it works:**
- Reset state to initial values
- Clear validation errors
- Reset touched state
- Can be partial or complete
- Can use HTML reset button

**Syntax:**
```tsx
const resetForm = () => {
  setFormData(initialState);
  setErrors({});
  setTouched({});
};
```

**Simple Example:**
```tsx
function ResettableForm() {
  const [value, setValue] = useState('');

  const resetForm = () => {
    setValue('');
  };

  return (
    <form>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <button type="button" onClick={resetForm}>Reset</button>
    </form>
  );
}
```

**Real-world Example:**
```tsx
interface FormData {
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
  address: string;
  city: string;
  state: string;
  zipCode: string;
  country: string;
  newsletter: boolean;
  terms: boolean;
}

const initialFormData: FormData = {
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  address: '',
  city: '',
  state: '',
  zipCode: '',
  country: '',
  newsletter: false,
  terms: false
};

function RegistrationFormWithReset() {
  const [formData, setFormData] = useState<FormData>(initialFormData);
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitSuccess, setSubmitSuccess] = useState(false);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value, type, checked } = e.target;
    const newValue = type === 'checkbox' ? checked : value;
    
    setFormData(prev => ({ ...prev, [name]: newValue }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    // Validate on blur
    const error = validateField(name, value);
    setErrors(prev => ({ ...prev, [name]: error || '' }));
  };

  const validateField = (name: string, value: string): string => {
    if (name === 'email' && !/\S+@\S+\.\S+/.test(value)) {
      return 'Invalid email format';
    }
    if (name === 'phone' && !/^\d{10}$/.test(value.replace(/\D/g, ''))) {
      return 'Invalid phone number';
    }
    if (name === 'zipCode' && !/^\d{5}(-\d{4})?$/.test(value)) {
      return 'Invalid ZIP code';
    }
    return '';
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};
    let isValid = true;

    (Object.keys(formData) as Array<keyof FormData>).forEach(field => {
      if (field === 'newsletter' || field === 'terms') return;
      
      const error = validateField(field, formData[field]);
      if (error) {
        newErrors[field] = error;
        isValid = false;
      }
    });

    setErrors(newErrors);
    return isValid;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) {
      setTouched({
        firstName: true,
        lastName: true,
        email: true,
        phone: true,
        address: true,
        city: true,
        state: true,
        zipCode: true,
        country: true
      });
      return;
    }

    setIsSubmitting(true);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Registration successful:', formData);
      setSubmitSuccess(true);
      
      // Auto-reset after success
      setTimeout(() => {
        resetForm();
        setSubmitSuccess(false);
      }, 3000);
    } catch (error) {
      console.error('Registration failed:', error);
    } finally {
      setIsSubmitting(false);
    }
  };

  const resetForm = () => {
    setFormData(initialFormData);
    setErrors({});
    setTouched({});
    setSubmitSuccess(false);
  };

  const clearForm = () => {
    setFormData({
      ...formData,
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
      address: '',
      city: '',
      state: '',
      zipCode: '',
      country: ''
    });
  };

  const isFormValid = Object.keys(errors).length === 0 &&
    Object.values(touched).filter(Boolean).length >= 3 &&
    formData.terms;

  const hasChanges = JSON.stringify(formData) !== JSON.stringify(initialFormData);

  return (
    <div className="registration-form">
      <h2>Registration Form</h2>

      {submitSuccess && (
        <div className="success-banner">
          <h3>Registration Successful!</h3>
          <p>Welcome aboard! You will be redirected shortly.</p>
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <div className="form-row">
          <div className="form-group">
            <label>First Name</label>
            <input
              type="text"
              name="firstName"
              value={formData.firstName}
              onChange={handleChange}
              onBlur={handleBlur}
              className={errors.firstName ? 'error' : ''}
            />
            {errors.firstName && <span className="error-text">{errors.firstName}</span>}
          </div>
          <div className="form-group">
            <label>Last Name</label>
            <input
              type="text"
              name="lastName"
              value={formData.lastName}
              onChange={handleChange}
              onBlur={handleBlur}
              className={errors.lastName ? 'error' : ''}
            />
            {errors.lastName && <span className="error-text">{errors.lastName}</span>}
          </div>
        </div>

        <div className="form-group">
          <label>Email</label>
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.email ? 'error' : ''}
          />
          {errors.email && <span className="error-text">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label>Phone</label>
          <input
            type="tel"
            name="phone"
            value={formData.phone}
            onChange={handleChange}
            onBlur={handleBlur}
            className={errors.phone ? 'error' : ''}
          />
          {errors.phone && <span className="error-text">{errors.phone}</span>}
        </div>

        <div className="form-group">
          <label>Address</label>
          <input
            type="text"
            name="address"
            value={formData.address}
            onChange={handleChange}
          />
        </div>

        <div className="form-row">
          <div className="form-group">
            <label>City</label>
            <input
              type="text"
              name="city"
              value={formData.city}
              onChange={handleChange}
            />
          </div>
          <div className="form-group">
            <label>State</label>
            <input
              type="text"
              name="state"
              value={formData.state}
              onChange={handleChange}
            />
          </div>
        </div>

        <div className="form-row">
          <div className="form-group">
            <label>ZIP Code</label>
            <input
              type="text"
              name="zipCode"
              value={formData.zipCode}
              onChange={handleChange}
              onBlur={handleBlur}
              className={errors.zipCode ? 'error' : ''}
            />
            {errors.zipCode && <span className="error-text">{errors.zipCode}</span>}
          </div>
          <div className="form-group">
            <label>Country</label>
            <input
              type="text"
              name="country"
              value={formData.country}
              onChange={handleChange}
            />
          </div>
        </div>

        <div className="form-group">
          <label>
            <input
              type="checkbox"
              name="newsletter"
              checked={formData.newsletter}
              onChange={handleChange}
            />
            Subscribe to newsletter
          </label>
        </div>

        <div className="form-group">
          <label>
            <input
              type="checkbox"
              name="terms"
              checked={formData.terms}
              onChange={handleChange}
            />
            I agree to the terms and conditions
          </label>
        </div>

        <div className="form-actions">
          <button type="button" onClick={clearForm} disabled={!hasChanges}>
            Clear Form
          </button>
          <button type="button" onClick={resetForm} disabled={!hasChanges}>
            Reset
          </button>
          <button type="submit" disabled={!isFormValid || isSubmitting}>
            {isSubmitting ? 'Registering...' : 'Register'}
          </button>
        </div>
      </form>
    </div>
  );
}
```

**Reset Patterns:**

1. **Complete reset:**
```tsx
const resetForm = () => {
  setFormData(initialState);
};
```

2. **Partial reset:**
```tsx
const clearFields = () => {
  setFormData({
    ...formData,
    field1: '',
    field2: ''
  });
};
```

3. **Reset with confirmation:**
```tsx
const handleReset = () => {
  if (confirm('Are you sure you want to reset?')) {
    resetForm();
  }
};
```

**Common Errors:**
- Not resetting validation state
- Not resetting touched state
- Not confirming destructive actions
- Losing user data accidentally
- Not providing feedback after reset

**Best Practices:**
- Reset all related state
- Provide confirmation for destructive actions
- Give feedback after reset
- Consider undo functionality
- Reset to appropriate initial state

---

## Rendering vs Event vs Side Effect

Understanding the difference between rendering, events, and side effects is crucial for writing correct React code and using useEffect properly.

### Rendering

**What is it?**
Rendering is the process React uses to display UI based on the current state and props. It's the core of React's declarative programming model.

**Why we need it:**
- Display UI to users
- React's primary purpose
- Declarative approach to UI
- Automatic and efficient
- Based on state/props changes

**How it works:**
- State or props change triggers render
- React creates virtual DOM
- Compares with previous virtual DOM
- Updates actual DOM efficiently
- Component function runs again

**Simple Example:**
```tsx
function Counter() {
  const [count, setCount] = useState(0);
  
  // This function runs on every render
  console.log('Component rendered');
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Real-world Example:**
```tsx
function ProductCard({ product, inStock }: { product: Product; inStock: boolean }) {
  // Rendering: Display UI based on props
  return (
    <div className={`product-card ${!inStock ? 'out-of-stock' : ''}`}>
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      {inStock ? (
        <button className="add-to-cart">Add to Cart</button>
      ) : (
        <button className="out-of-stock" disabled>Out of Stock</button>
      )}
    </div>
  );
}
```

**Rendering Characteristics:**
- **Pure:** Same inputs = same outputs
- **Predictable:** Based on state/props
- **Efficient:** Optimized by React
- **Declarative:** Describe what to show
- **Automatic:** React handles updates

---

### Event

**What is it?**
Events are user or browser interactions that trigger callback functions, allowing React components to respond to user actions.

**Why we need it:**
- Handle user interactions
- Respond to browser events
- Enable interactive UIs
- Capture user input
- Trigger state changes

**How it works:**
- User performs action (click, change, submit)
- React synthetic event fires
- Event handler function executes
- State can be updated
- Re-render may be triggered

**Simple Example:**
```tsx
function Button() {
  const [clicked, setClicked] = useState(false);
  
  const handleClick = (event: React.MouseEvent) => {
    console.log('Button clicked!', event);
    setClicked(true);
  };
  
  return (
    <button onClick={handleClick}>
      {clicked ? 'Clicked!' : 'Click me'}
    </button>
  );
}
```

**Real-world Example:**
```tsx
function InteractiveCard() {
  const [isExpanded, setIsExpanded] = useState(false);
  const [likes, setLikes] = useState(0);

  const handleExpand = (event: React.MouseEvent) => {
    event.stopPropagation(); // Prevent event bubbling
    setIsExpanded(!isExpanded);
  };

  const handleLike = (event: React.MouseEvent) => {
    event.stopPropagation();
    setLikes(prev => prev + 1);
  };

  const handleShare = (event: React.MouseEvent) => {
    event.stopPropagation();
    // Share functionality
    console.log('Shared!');
  };

  return (
    <div className="interactive-card" onClick={handleExpand}>
      <div className="card-header">
        <h3>Card Title</h3>
        <button onClick={handleShare}>Share</button>
      </div>
      
      {isExpanded && (
        <div className="card-content">
          <p>Expanded content goes here</p>
          <button onClick={handleLike}>
            Like ({likes})
          </button>
        </div>
      )}
    </div>
  );
}
```

**Event Characteristics:**
- **User-initiated:** Triggered by user actions
- **Immediate:** Happens when action occurs
- **Interactive:** Enables user communication
- **Optional:** Components can ignore events
- **Bubbling:** Events propagate through DOM tree

---

### Side Effect

**What is it?**
Side effects are operations that affect things outside the component's rendering, such as API calls, DOM manipulation, timers, or subscriptions.

**Why we need it:**
- Interact with external systems
- Perform I/O operations
- Update browser APIs
- Set up timers/subscriptions
- Integrate with third-party libraries

**How it works:**
- Don't happen during rendering
- Handle by useEffect
- Cleanup when component unmounts
- Can depend on state/props
- Run at specific times

**Simple Example:**
```tsx
import { useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Side effect: setInterval
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // Cleanup function
    return () => clearInterval(interval);
  }, []);

  return <div>Seconds: {seconds}</div>;
}
```

**Real-world Example:**
```tsx
function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Side effect: Fetch data from API
    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        setUser(userData);
      } catch (err) {
        setError('Failed to load user');
        console.error(err);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]); // Re-run when userId changes

  // Another side effect: Document title
  useEffect(() => {
    if (user) {
      document.title = `${user.name}'s Profile`;
    }
    return () => {
      document.title = 'My App'; // Cleanup
    };
  }, [user]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

**Side Effect Characteristics:**
- **External:** Affects outside world
- **Async:** Often I/O operations
- **Cleanup:** May need cleanup
- **Timing:** Run at specific times
- **Not rendering:** Don't happen during render

---

### Comparison Table

| Aspect | Rendering | Event | Side Effect |
|--------|-----------|-------|-------------|
| **Purpose** | Display UI | Handle user interaction | External operations |
| **Trigger** | State/props change | User action | Component lifecycle |
| **Timing** | During render | Immediately when action occurs | After render, on specific conditions |
| **Return** | JSX/UI | Event handler function | Cleanup function |
| **Use Case** | Show content | Interactive features | API calls, timers, subscriptions |
| **React Hook** | N/A (component function) | useState for state | useEffect for side effects |

### Combined Example

```tsx
function DataComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [filter, setFilter] = useState('all');

  // Side effect: Fetch data when component mounts
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('/api/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Failed to fetch data:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  // Side effect: Update document title when data changes
  useEffect(() => {
    if (data) {
      document.title = `Data Viewer - ${data.length} items`;
    }
  }, [data]);

  // Rendering: Display UI based on state
  if (loading) return <div>Loading...</div>;

  const filteredData = filter === 'all' 
    ? data 
    : data.filter(item => item.category === filter);

  return (
    <div>
      {/* Event: User interaction */}
      <select 
        value={filter} 
        onChange={(e) => setFilter(e.target.value)}
      >
        <option value="all">All</option>
        <option value="electronics">Electronics</option>
        <option value="clothing">Clothing</option>
      </select>

      {/* Rendering: Display filtered data */}
      <ul>
        {filteredData.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### Key Takeaways

- **Rendering:** Display UI based on state/props - happens automatically
- **Events:** Handle user interactions - triggered by user actions
- **Side Effects:** External operations - handled by useEffect, not during rendering

Understanding these differences is crucial for knowing when to use useState for state/event handling and when to use useEffect for side effects.

---

## Part 3: useEffect

### 1. What is useEffect?

**What is it?**
useEffect is a React hook that lets you perform side effects in functional components, such as data fetching, subscriptions, or manually changing the DOM.

**Why we need it?**
- Handle side effects in functional components
- Replace lifecycle methods from class components
- Perform operations outside rendering
- Interact with external systems
- Clean up resources

**How it works?**
- Runs after render completes
- Can run on every render or conditionally
- Accepts a dependency array
- Can return a cleanup function
- Schedules effects, doesn't block rendering

**Syntax:**
```tsx
useEffect(() => {
  // Effect code here
  return () => {
    // Cleanup code here
  };
}, [dependencies]);
```

**Simple Example:**
```tsx
import { useEffect, useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Side effect: Update document title
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Real-world Example:**
```tsx
import { useEffect, useState } from 'react';

interface User {
  id: number;
  name: string;
  email: string;
  avatar: string;
}

function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // Side effect: Fetch user data
    const fetchUser = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!response.ok) {
          throw new Error('Failed to fetch user');
        }
        
        const userData: User = await response.json();
        setUser(userData);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'An error occurred');
        console.error('Error fetching user:', err);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]); // Re-run when userId changes

  // Another effect: Update document title
  useEffect(() => {
    if (user) {
      document.title = `${user.name}'s Profile`;
    }
    return () => {
      document.title = 'My App'; // Cleanup: Reset title
    };
  }, [user]);

  if (loading) return <div className="loading">Loading user profile...</div>;
  if (error) return <div className="error">Error: {error}</div>;
  if (!user) return <div className="not-found">User not found</div>;

  return (
    <div className="user-profile">
      <div className="profile-header">
        <img src={user.avatar} alt={user.name} className="avatar" />
        <div className="profile-info">
          <h1>{user.name}</h1>
          <p className="email">{user.email}</p>
        </div>
      </div>
      <div className="profile-content">
        <h2>User Details</h2>
        <p><strong>ID:</strong> {user.id}</p>
        <p><strong>Name:</strong> {user.name}</p>
        <p><strong>Email:</strong> {user.email}</p>
      </div>
    </div>
  );
}
```

**useEffect Characteristics:**
- **Runs after render:** Not during rendering
- **Can be conditional:** Based on dependencies
- **Cleanup support:** Return cleanup function
- **Multiple effects:** Can have multiple useEffect hooks
- **Async-friendly:** Can handle async operations

---

### 2. Why useEffect?

**What is it?**
Understanding the purpose and importance of useEffect in React applications and why it's necessary for functional components.

**Why we need it?**
- Functional components need lifecycle capabilities
- Handle side effects properly
- Avoid doing work during rendering
- Prevent memory leaks with cleanup
- Follow React best practices

**How it works?**
- Replaces class component lifecycle methods
- Provides declarative way to handle effects
- Separates rendering from side effects
- Manages effect dependencies automatically
- Ensures proper cleanup

**Lifecycle Method Mapping:**

| Class Component | useEffect Equivalent |
|----------------|---------------------|
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [deps])` |
| `componentWillUnmount` | Return cleanup function in useEffect |

**Simple Example - Without useEffect (Problem):**
```tsx
// ❌ WRONG: Doing side effects during render
function Timer() {
  const [count, setCount] = useState(0);
  
  // This runs on every render - causes issues!
  document.title = `Count: ${count}`;
  
  return <div>{count}</div>;
}
```

**Simple Example - With useEffect (Correct):**
```tsx
// ✅ CORRECT: Using useEffect for side effects
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    // Side effect runs after render
    document.title = `Count: ${count}`;
  }, [count]);
  
  return <div>{count}</div>;
}
```

**Real-world Example:**
```tsx
// Class component (old way)
class DataFetcher extends React.Component {
  state = {
    data: null,
    loading: true,
    error: null
  };

  componentDidMount() {
    this.fetchData();
  }

  componentDidUpdate(prevProps) {
    if (this.props.userId !== prevProps.userId) {
      this.fetchData();
    }
  }

  componentWillUnmount() {
    // Cleanup
    if (this.abortController) {
      this.abortController.abort();
    }
  }

  fetchData = async () => {
    this.abortController = new AbortController();
    
    try {
      this.setState({ loading: true, error: null });
      const response = await fetch(`/api/users/${this.props.userId}`, {
        signal: this.abortController.signal
      });
      const data = await response.json();
      this.setState({ data, loading: false });
    } catch (error) {
      if (!error.name === 'AbortError') {
        this.setState({ error: error.message, loading: false });
      }
    }
  };

  render() {
    // ... render logic
  }
}

// Functional component with useEffect (new way)
function DataFetcher({ userId }: { userId: number }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const abortController = new AbortController();

    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        const response = await fetch(`/api/users/${userId}`, {
          signal: abortController.signal
        });
        const result = await response.json();
        setData(result);
      } catch (err) {
        if (!err.name === 'AbortError') {
          setError(err.message);
        }
      } finally {
        setLoading(false);
      }
    };

    fetchData();

    // Cleanup function (componentWillUnmount)
    return () => {
      abortController.abort();
    };
  }, [userId]); // Re-run when userId changes (componentDidUpdate)

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <div>{/* render data */}</div>;
}
```

**Why useEffect is Important:**

1. **Separation of Concerns:**
   - Rendering is for displaying UI
   - useEffect is for side effects
   - Clear separation makes code predictable

2. **Performance:**
   - Effects run after render
   - Don't block rendering
   - Can be skipped with dependencies

3. **Memory Management:**
   - Cleanup functions prevent leaks
   - Automatic cleanup on unmount
   - Proper resource management

4. **Declarative:**
   - Describe what to do, not when
   - React handles timing
   - Less error-prone

5. **Reusable:**
   - Custom hooks can use useEffect
   - Share effect logic
   - Composable patterns

**Common Scenarios Requiring useEffect:**
- Data fetching
- Subscriptions (WebSocket, EventSource)
- Timers (setTimeout, setInterval)
- DOM manipulation
- Browser API calls
- Third-party library integration

---

### 3. Side Effects

**What is it?**
Operations that affect things outside the component's rendering, such as network requests, DOM manipulation, or browser API interactions.

**Why we need it?**
- Interact with external systems
- Perform I/O operations
- Update browser state
- Integrate with third-party libraries
- Handle asynchronous operations

**How it works?**
- Happen outside rendering
- Handled by useEffect
- May require cleanup
- Can depend on state/props
- Run at specific times

**Types of Side Effects:**

1. **Data Fetching:** API calls, GraphQL queries
2. **Subscriptions:** WebSocket, EventSource
3. **Timers:** setTimeout, setInterval
4. **DOM Manipulation:** Direct DOM changes
5. **Browser APIs:** localStorage, sessionStorage
6. **Third-party Libraries:** Chart.js, Google Maps
7. **Event Listeners:** Window, document events

**Simple Example:**
```tsx
function DataFetcher() {
  const [data, setData] = useState(null);

  useEffect(() => {
    // Side effect: Fetch data
    fetch('/api/data')
      .then(response => response.json())
      .then(result => setData(result));
  }, []);

  return <div>{data ? data.name : 'Loading...'}</div>;
}
```

**Real-world Example:**
```tsx
function InteractiveMap() {
  const [map, setMap] = useState(null);
  const [userLocation, setUserLocation] = useState(null);
  const [events, setEvents] = useState([]);

  // Side effect 1: Initialize map
  useEffect(() => {
    // Third-party library integration
    const mapInstance = new MapLibreGL.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [-74.5, 40],
      zoom: 9
    });

    setMap(mapInstance);

    // Cleanup: Remove map
    return () => {
      mapInstance.remove();
    };
  }, []);

  // Side effect 2: Get user location
  useEffect(() => {
    // Browser API call
    if (navigator.geolocation) {
      const watchId = navigator.geolocation.watchPosition(
        (position) => {
          setUserLocation({
            lat: position.coords.latitude,
            lng: position.coords.longitude
          });
        },
        (error) => {
          console.error('Geolocation error:', error);
        }
      );

      // Cleanup: Stop watching location
      return () => {
        navigator.geolocation.clearWatch(watchId);
      };
    }
  }, []);

  // Side effect 3: Track events on map
  useEffect(() => {
    if (!map) return;

    const handleMapClick = (e) => {
      console.log('Map clicked:', e.lngLat);
      setEvents(prev => [...prev, { type: 'click', data: e.lngLat }]);
    };

    const handleMapMove = () => {
      console.log('Map moved');
      setEvents(prev => [...prev, { type: 'move', data: map.getCenter() }]);
    };

    // Event listeners
    map.on('click', handleMapClick);
    map.on('move', handleMapMove);

    // Cleanup: Remove event listeners
    return () => {
      map.off('click', handleMapClick);
      map.off('move', handleMapMove);
    };
  }, [map]);

  // Side effect 4: Update map when user location changes
  useEffect(() => {
    if (map && userLocation) {
      map.flyTo({
        center: [userLocation.lng, userLocation.lat],
        zoom: 14
      });
    }
  }, [map, userLocation]);

  return (
    <div>
      <div id="map" style={{ width: '100%', height: '400px' }}></div>
      {userLocation && (
        <div className="location-info">
          <p>Location: {userLocation.lat}, {userLocation.lng}</p>
        </div>
      )}
      <div className="event-log">
        <h3>Event Log</h3>
        <ul>
          {events.map((event, index) => (
            <li key={index}>
              {event.type}: {JSON.stringify(event.data)}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}
```

**Side Effect Patterns:**

1. **One-time effect (mount):**
```tsx
useEffect(() => {
  // Runs once on mount
  console.log('Component mounted');
}, []);
```

2. **Effect with dependencies:**
```tsx
useEffect(() => {
  // Runs when data changes
  console.log('Data changed:', data);
}, [data]);
```

3. **Effect with cleanup:**
```tsx
useEffect(() => {
  const subscription = api.subscribe();
  return () => subscription.unsubscribe();
}, []);
```

**Common Errors:**
- Doing side effects during render
- Not cleaning up resources
- Missing cleanup for subscriptions
- Not handling async errors
- Incorrect dependency arrays

**Best Practices:**
- Always use useEffect for side effects
- Provide cleanup when needed
- Handle errors properly
- Use correct dependencies
- Keep effects focused

---

### 4. useEffect syntax

**What is it?**
The syntax and structure of the useEffect hook, including the effect function, cleanup function, and dependency array.

**Why we need it?**
- Understand how to write effects
- Know when effects run
- Control effect execution
- Implement cleanup properly
- Avoid common mistakes

**How it works?**
- Accepts a function (effect)
- Optionally returns a cleanup function
- Accepts a dependency array
- Runs based on dependencies
- Handles cleanup automatically

**Syntax:**
```tsx
useEffect(effectFunction, dependencyArray);
```

**Full Syntax:**
```tsx
useEffect(() => {
  // Effect code - runs after render
  
  // Optional: Return cleanup function
  return () => {
    // Cleanup code - runs before unmount or before re-run
  };
}, [dependency1, dependency2]); // Dependency array
```

**Syntax Variations:**

1. **No dependency array (runs every render):**
```tsx
useEffect(() => {
  console.log('Runs on every render');
});
```

2. **Empty dependency array (runs once on mount):**
```tsx
useEffect(() => {
  console.log('Runs once on mount');
}, []);
```

3. **With dependencies (runs when dependencies change):**
```tsx
useEffect(() => {
  console.log('Runs when count changes');
}, [count]);
```

4. **With cleanup:**
```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Timer tick');
  }, 1000);
  
  return () => {
    clearInterval(timer); // Cleanup
  };
}, []);
```

**Simple Example:**
```tsx
function Component() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Effect: Update document title
    document.title = `Count: ${count}`;
    
    // Cleanup: Reset title on unmount
    return () => {
      document.title = 'My App';
    };
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

**Real-world Example:**
```tsx
function NewsFeed({ category }: { category: string }) {
  const [articles, setArticles] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Effect function
    const fetchArticles = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(`/api/articles?category=${category}`);
        
        if (!response.ok) {
          throw new Error('Failed to fetch articles');
        }
        
        const data = await response.json();
        setArticles(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'An error occurred');
      } finally {
        setLoading(false);
      }
    };

    fetchArticles();

    // Cleanup function (optional)
    return () => {
      // Cancel any pending requests
      // Reset loading state if needed
      console.log('Cleaning up article fetch');
    };
  }, [category]); // Dependency array

  return (
    <div>
      <h2>{category} News</h2>
      {loading && <div>Loading articles...</div>}
      {error && <div>Error: {error}</div>}
      <ul>
        {articles.map(article => (
          <li key={article.id}>{article.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Syntax Breakdown:**

```tsx
useEffect(
  // 1. Effect function (required)
  () => {
    // Code to run after render
    console.log('Effect ran');
    
    // Can include async operations
    fetchData();
    
    // Can set state
    setState(newValue);
    
    // 2. Cleanup function (optional)
    return () => {
      // Code to run before unmount or before re-run
      console.log('Cleanup ran');
      cleanupResources();
    };
  },
  // 3. Dependency array (optional but recommended)
  [dependency1, dependency2]
);
```

**Common Syntax Mistakes:**

1. **❌ Wrong: Async function directly in useEffect**
```tsx
useEffect(async () => {
  const data = await fetch('/api/data');
  setData(data);
}, []);
```

2. **✅ Correct: Async function inside useEffect**
```tsx
useEffect(() => {
  const fetchData = async () => {
    const data = await fetch('/api/data');
    setData(data);
  };
  fetchData();
}, []);
```

3. **❌ Wrong: Missing dependency array**
```tsx
useEffect(() => {
  document.title = `Count: ${count}`; // count not in dependencies
});
```

4. **✅ Correct: Include dependencies**
```tsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

**Best Practices:**
- Always include dependency array
- Use cleanup for resources
- Don't make effect function async
- Keep effects focused
- Extract complex effects to functions

---

### 5. Empty dependency array

**What is it?**
An empty dependency array `[]` in useEffect causes the effect to run only once when the component mounts, similar to componentDidMount.

**Why we need it?**
- Run effects only on mount
- One-time setup operations
- Initialize resources
- Fetch initial data
- Set up subscriptions

**How it works?**
- Effect runs on first render (mount)
- Doesn't run on subsequent renders
- Cleanup runs on unmount
- Equivalent to componentDidMount
- No dependency tracking

**Syntax:**
```tsx
useEffect(() => {
  // Runs once on mount
  console.log('Component mounted');
}, []); // Empty dependency array
```

**Simple Example:**
```tsx
function Component() {
  useEffect(() => {
    console.log('Component mounted');
    
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  return <div>Hello</div>;
}
```

**Real-world Example:**
```tsx
function UserProfile() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Runs once on mount - fetch initial user data
    const fetchInitialUser = async () => {
      try {
        setLoading(true);
        const response = await fetch('/api/user/current');
        const userData = await response.json();
        setUser(userData);
      } catch (error) {
        console.error('Failed to fetch user:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchInitialUser();
  }, []); // Empty array = run once on mount

  if (loading) return <div>Loading...</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

**When to Use Empty Dependency Array:**

1. **One-time data fetch:**
```tsx
useEffect(() => {
  fetch('/api/initial-data').then(setData);
}, []);
```

2. **Initialize third-party library:**
```tsx
useEffect(() => {
  const chart = new Chart(canvasRef.current, config);
  return () => chart.destroy();
}, []);
```

3. **Set up browser API:**
```tsx
useEffect(() => {
  requestAnimationFrame(() => {
    // One-time animation setup
  });
}, []);
```

4. **Initialize analytics:**
```tsx
useEffect(() => {
  analytics.init();
  analytics.track('page_view');
}, []);
```

**Common Mistakes:**

1. **❌ Using empty array when effect should re-run:**
```tsx
useEffect(() => {
  // Should re-run when userId changes
  fetch(`/api/users/${userId}`).then(setUser);
}, []); // Wrong - empty array
```

2. **✅ Correct: Include dependency:**
```tsx
useEffect(() => {
  fetch(`/api/users/${userId}`).then(setUser);
}, [userId]); // Correct - include userId
```

3. **❌ Not handling stale data:**
```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log(count); // count might be stale
  }, 1000);
  return () => clearInterval(timer);
}, []);
```

4. **✅ Correct: Use functional updates:**
```tsx
useEffect(() => {
  const timer = setInterval(() => {
    setCount(prev => prev + 1); // Always uses latest count
  }, 1000);
  return () => clearInterval(timer);
}, []);
```

**Best Practices:**
- Use for one-time setup operations
- Ensure effect doesn't depend on changing values
- Provide cleanup if needed
- Consider if effect should re-run
- Document why empty array is used

---

### 6. Dependencies

**What is it?**
The dependency array in useEffect that specifies which values the effect depends on, controlling when the effect re-runs.

**Why we need it?**
- Control when effects run
- Prevent unnecessary re-runs
- Ensure effects have latest data
- Optimize performance
- Follow React's rules

**How it works?**
- React compares dependencies to previous values
- Effect re-runs when any dependency changes
- Missing dependencies cause stale closures
- Extra dependencies cause unnecessary re-runs
- ESLint can detect missing dependencies

**Syntax:**
```tsx
useEffect(() => {
  // Effect code
}, [dep1, dep2, dep3]); // Dependencies
```

**Simple Example:**
```tsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]); // Re-run when count changes

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

**Real-world Example:**
```tsx
function SearchResults({ query, category, sortBy }: { query: string; category: string; sortBy: string }) {
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    // Effect depends on query, category, and sortBy
    const search = async () => {
      try {
        setLoading(true);
        const params = new URLSearchParams({
          q: query,
          category,
          sort: sortBy
        });
        
        const response = await fetch(`/api/search?${params}`);
        const data = await response.json();
        setResults(data);
      } catch (error) {
        console.error('Search failed:', error);
      } finally {
        setLoading(false);
      }
    };

    search();
  }, [query, category, sortBy]); // Re-run when any of these change

  return (
    <div>
      {loading && <div>Searching...</div>}
      <ul>
        {results.map(item => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Dependency Patterns:**

1. **Single dependency:**
```tsx
useEffect(() => {
  console.log('User changed:', user);
}, [user]);
```

2. **Multiple dependencies:**
```tsx
useEffect(() => {
  console.log('Filters changed:', { filter, sort, page });
}, [filter, sort, page]);
```

3. **Object/array dependencies:**
```tsx
useEffect(() => {
  console.log('Config changed:', config);
}, [config]); // Note: Reference equality
```

4. **Derived values:**
```tsx
const derivedValue = useMemo(() => compute(value), [value]);

useEffect(() => {
  console.log('Derived changed:', derivedValue);
}, [derivedValue]);
```

**Common Dependency Mistakes:**

1. **❌ Missing dependency:**
```tsx
useEffect(() => {
  console.log(count); // count not in dependencies
}, []);
```

2. **✅ Correct: Include dependency:**
```tsx
useEffect(() => {
  console.log(count);
}, [count]);
```

3. **❌ Too many dependencies:**
```tsx
useEffect(() => {
  fetchData();
}, [user, settings, config, theme, locale]); // Too many
```

4. **✅ Correct: Extract what's needed:**
```tsx
const userId = user.id;
const theme = settings.theme;

useEffect(() => {
  fetchData(userId, theme);
}, [userId, theme]);
```

5. **❌ Object reference causing re-runs:**
```tsx
const config = { name: 'test' }; // New reference every render

useEffect(() => {
  console.log(config);
}, [config]); // Re-runs every render
```

6. **✅ Correct: useMemo or stable reference:**
```tsx
const config = useMemo(() => ({ name: 'test' }), []);

useEffect(() => {
  console.log(config);
}, [config]);
```

**Best Practices:**
- Include all values used in effect
- Remove unused dependencies
- Use ESLint to detect issues
- Consider performance impact
- Document dependency decisions

---

### 7. Effect execution

**What is it?**
Understanding when and how useEffect effects execute in the component lifecycle, including timing and order of execution.

**Why we need it?**
- Predict effect behavior
- Debug timing issues
- Understand React's rendering
- Optimize performance
- Avoid race conditions

**How it works?**
- Effects run after render completes
- React batches effects
- Multiple effects run in order
- Cleanup runs before next effect
- Guarantees DOM is updated

**Execution Timeline:**

```
1. Component renders
2. DOM updates
3. useEffect effects run (in order)
4. Cleanup from previous effects runs (if any)
5. Browser paints
```

**Simple Example:**
```tsx
function EffectDemo() {
  const [count, setCount] = useState(0);

  console.log('1. Component render');

  useEffect(() => {
    console.log('2. Effect ran');
  });

  useEffect(() => {
    console.log('3. Second effect ran');
  }, [count]);

  return <div>{count}</div>;
}
```

**Real-world Example:**
```tsx
function DataVisualization({ datasetId }: { datasetId: number }) {
  const [data, setData] = useState(null);
  const [chart, setChart] = useState(null);

  console.log('Render started');

  // Effect 1: Fetch data
  useEffect(() => {
    console.log('Effect 1: Fetching data');
    
    const fetchData = async () => {
      const response = await fetch(`/api/datasets/${datasetId}`);
      const result = await response.json();
      setData(result);
    };

    fetchData();
  }, [datasetId]);

  // Effect 2: Initialize chart
  useEffect(() => {
    console.log('Effect 2: Initializing chart');
    
    if (data && !chart) {
      const chartInstance = new Chart(canvasRef.current, {
        type: 'bar',
        data: data
      });
      setChart(chartInstance);
    }
  }, [data, chart]);

  // Effect 3: Update chart when data changes
  useEffect(() => {
    console.log('Effect 3: Updating chart');
    
    if (chart && data) {
      chart.data = data;
      chart.update();
    }
  }, [chart, data]);

  console.log('Render completed');

  return <canvas ref={canvasRef} />;
}
```

**Execution Order:**

```tsx
function OrderDemo() {
  console.log('1. Render start');

  const [value, setValue] = useState(0);

  console.log('2. After useState');

  useEffect(() => {
    console.log('3. Effect 1');
  }, []);

  useEffect(() => {
    console.log('4. Effect 2');
  }, [value]);

  console.log('5. Render end');

  return <div>{value}</div>;
}

// First render output:
// 1. Render start
// 2. After useState
// 5. Render end
// 3. Effect 1
// 4. Effect 2
```

**Effect Execution Rules:**

1. **After every render (no deps):**
```tsx
useEffect(() => {
  console.log('Runs after every render');
});
```

2. **After mount (empty deps):**
```tsx
useEffect(() => {
  console.log('Runs once after mount');
}, []);
```

3. **After dependency changes:**
```tsx
useEffect(() => {
  console.log('Runs when value changes');
}, [value]);
```

**Common Issues:**

1. **❌ Assuming synchronous execution:**
```tsx
useEffect(() => {
  fetchData();
  console.log(data); // data not updated yet!
}, []);
```

2. **✅ Correct: Use effect or derived state:**
```tsx
useEffect(() => {
  fetchData();
}, []);

useEffect(() => {
  console.log(data); // Runs when data updates
}, [data]);
```

3. **❌ Multiple effects depending on each other:**
```tsx
useEffect(() => {
  setData1(newValue);
}, []);

useEffect(() => {
  setData2(data1 + 1); // data1 might not be ready
}, [data1]);
```

4. **✅ Correct: Combine or use sequential effects:**
```tsx
useEffect(() => {
  const newValue = compute();
  setData1(newValue);
  setData2(newValue + 1);
}, []);
```

**Best Practices:**
- Effects run after render, not during
- DOM is updated before effects run
- Multiple effects run in declaration order
- Cleanup runs before next effect
- Don't assume synchronous updates

---

### 8. Cleanup

**What is it?**
The cleanup function returned by useEffect that runs before the component unmounts or before the effect re-runs, used to clean up resources and prevent memory leaks.

**Why we need it?**
- Prevent memory leaks
- Clean up resources
- Remove event listeners
- Cancel async operations
- Reset external state

**How it works?**
- Return a function from useEffect
- Runs before unmount
- Runs before effect re-runs
- Cleanup previous effect state
- Automatically called by React

**Syntax:**
```tsx
useEffect(() => {
  // Setup code
  const resource = createResource();
  
  return () => {
    // Cleanup code
    resource.cleanup();
  };
}, [dependencies]);
```

**Simple Example:**
```tsx
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Setup: Create interval
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // Cleanup: Clear interval
    return () => clearInterval(interval);
  }, []);

  return <div>Seconds: {seconds}</div>;
}
```

**Real-world Example:**
```tsx
function ChatRoom({ roomId }: { roomId: string }) {
  const [messages, setMessages] = useState([]);
  const [connection, setConnection] = useState(null);

  useEffect(() => {
    // Setup: Connect to chat room
    const ws = new WebSocket(`wss://api.example.com/chat/${roomId}`);
    
    ws.onopen = () => {
      console.log('Connected to chat room:', roomId);
    };

    ws.onmessage = (event) => {
      const message = JSON.parse(event.data);
      setMessages(prev => [...prev, message]);
    };

    ws.onerror = (error) => {
      console.error('WebSocket error:', error);
    };

    setConnection(ws);

    // Cleanup: Close connection
    return () => {
      console.log('Disconnecting from chat room:', roomId);
      ws.close();
    };
  }, [roomId]);

  return (
    <div>
      <h2>Chat Room: {roomId}</h2>
      <ul>
        {messages.map((msg, index) => (
          <li key={index}>{msg.text}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Cleanup Scenarios:**

1. **Timers:**
```tsx
useEffect(() => {
  const timeout = setTimeout(() => {
    console.log('Timeout fired');
  }, 1000);
  
  return () => clearTimeout(timeout);
}, []);
```

2. **Event listeners:**
```tsx
useEffect(() => {
  const handleResize = () => {
    console.log('Window resized');
  };
  
  window.addEventListener('resize', handleResize);
  
  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);
```

3. **Subscriptions:**
```tsx
useEffect(() => {
  const subscription = api.subscribe(data => {
    console.log('Data received:', data);
  });
  
  return () => subscription.unsubscribe();
}, []);
```

4. **Third-party libraries:**
```tsx
useEffect(() => {
  const chart = new Chart(canvasRef.current, config);
  
  return () => chart.destroy();
}, []);
```

**Common Cleanup Mistakes:**

1. **❌ Not cleaning up:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log('tick');
  }, 1000);
  // No cleanup - memory leak!
}, []);
```

2. **✅ Correct: Cleanup properly:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log('tick');
  }, 1000);
  
  return () => clearInterval(interval);
}, []);
```

3. **❌ Cleanup in wrong place:**
```tsx
useEffect(() => {
  fetch('/api/data').then(setData);
  return () => {
    // Can't cancel fetch like this
    cancelFetch();
  };
}, []);
```

4. **✅ Correct: Use AbortController:**
```tsx
useEffect(() => {
  const controller = new AbortController();
  
  fetch('/api/data', { signal: controller.signal })
    .then(setData);
  
  return () => controller.abort();
}, []);
```

**Best Practices:**
- Always cleanup when needed
- Use AbortController for fetch
- Remove event listeners
- Clear timers and intervals
- Close connections and subscriptions

---

### 9. setInterval

**What is it?**
Using setInterval within useEffect to create repeating timers that execute code at regular intervals.

**Why we need it?**
- Create periodic updates
- Poll for data
- Implement countdowns
- Real-time updates
- Background tasks

**How it works?**
- Create interval in useEffect
- Store interval ID
- Update state in callback
- Clear interval in cleanup
- Use functional updates

**Syntax:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    // Code to run periodically
  }, intervalTime);
  
  return () => clearInterval(interval);
}, [dependencies]);
```

**Simple Example:**
```tsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(prev => prev + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return <div>Count: {count}</div>;
}
```

**Real-world Example:**
```tsx
function CountdownTimer({ targetDate }: { targetDate: Date }) {
  const [timeLeft, setTimeLeft] = useState({
    days: 0,
    hours: 0,
    minutes: 0,
    seconds: 0
  });

  useEffect(() => {
    const calculateTimeLeft = () => {
      const difference = targetDate.getTime() - new Date().getTime();
      
      if (difference <= 0) {
        return { days: 0, hours: 0, minutes: 0, seconds: 0 };
      }

      return {
        days: Math.floor(difference / (1000 * 60 * 60 * 24)),
        hours: Math.floor((difference / (1000 * 60 * 60)) % 24),
        minutes: Math.floor((difference / 1000 / 60) % 60),
        seconds: Math.floor((difference / 1000) % 60)
      };
    };

    setTimeLeft(calculateTimeLeft());

    const interval = setInterval(() => {
      setTimeLeft(calculateTimeLeft());
    }, 1000);

    return () => clearInterval(interval);
  }, [targetDate]);

  const isExpired = timeLeft.days === 0 && 
                    timeLeft.hours === 0 && 
                    timeLeft.minutes === 0 && 
                    timeLeft.seconds === 0;

  return (
    <div className="countdown-timer">
      {isExpired ? (
        <div className="expired">Time's up!</div>
      ) : (
        <div className="timer-display">
          <div className="time-unit">
            <span className="value">{timeLeft.days}</span>
            <span className="label">Days</span>
          </div>
          <div className="time-unit">
            <span className="value">{timeLeft.hours}</span>
            <span className="label">Hours</span>
          </div>
          <div className="time-unit">
            <span className="value">{timeLeft.minutes}</span>
            <span className="label">Minutes</span>
          </div>
          <div className="time-unit">
            <span className="value">{timeLeft.seconds}</span>
            <span className="label">Seconds</span>
          </div>
        </div>
      )}
    </div>
  );
}
```

**setInterval Patterns:**

1. **Simple counter:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

2. **Conditional interval:**
```tsx
useEffect(() => {
  if (!isRunning) return;
  
  const interval = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);
  
  return () => clearInterval(interval);
}, [isRunning]);
```

3. **Dynamic interval time:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    // ... update logic
  }, intervalTime);
  
  return () => clearInterval(interval);
}, [intervalTime]);
```

**Common Mistakes:**

1. **❌ Not using functional updates:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    setCount(count + 1); // Stale closure!
  }, 1000);
  return () => clearInterval(interval);
}, []); // count not in deps
```

2. **✅ Correct: Functional updates:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1); // Always latest
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

3. **❌ Not cleaning up:**
```tsx
useEffect(() => {
  setInterval(() => {
    console.log('tick');
  }, 1000);
  // No cleanup - memory leak!
}, []);
```

4. **✅ Correct: Cleanup interval:**
```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log('tick');
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

**Best Practices:**
- Always use functional updates
- Always cleanup intervals
- Consider dynamic interval times
- Handle edge cases (expired, paused)
- Test memory usage

---

### 10. Event listeners

**What is it?**
Adding and removing event listeners using useEffect to handle browser events like clicks, scrolls, and keyboard events.

**Why we need it?**
- Handle global events
- Respond to user actions
- Integrate with browser APIs
- Create interactive features
- Custom event handling

**How it works?**
- Add listener in useEffect
- Store handler function
- Remove listener in cleanup
- Handle event appropriately
- Use stable references

**Syntax:**
```tsx
useEffect(() => {
  const handleEvent = (event) => {
    // Handle event
  };
  
  window.addEventListener('eventType', handleEvent);
  
  return () => {
    window.removeEventListener('eventType', handleEvent);
  };
}, [dependencies]);
```

**Simple Example:**
```tsx
function WindowSize() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return <div>Window width: {width}px</div>;
}
```

**Real-world Example:**
```tsx
function KeyboardShortcuts() {
  const [pressedKeys, setPressedKeys] = useState<Set<string>>(new Set());

  useEffect(() => {
    const handleKeyDown = (event: KeyboardEvent) => {
      setPressedKeys(prev => new Set(prev).add(event.key));
      
      // Handle specific shortcuts
      if (event.ctrlKey && event.key === 's') {
        event.preventDefault();
        console.log('Save shortcut triggered');
      }
      
      if (event.key === 'Escape') {
        console.log('Escape pressed');
      }
    };

    const handleKeyUp = (event: KeyboardEvent) => {
      setPressedKeys(prev => {
        const newSet = new Set(prev);
        newSet.delete(event.key);
        return newSet;
      });
    };

    window.addEventListener('keydown', handleKeyDown);
    window.addEventListener('keyup', handleKeyUp);

    return () => {
      window.removeEventListener('keydown', handleKeyDown);
      window.removeEventListener('keyup', handleKeyUp);
    };
  }, []);

  return (
    <div className="keyboard-shortcuts">
      <h3>Keyboard Shortcuts</h3>
      <div className="pressed-keys">
        <p>Pressed keys: {Array.from(pressedKeys).join(', ') || 'None'}</p>
      </div>
      <div className="shortcuts-list">
        <p><kbd>Ctrl</kbd> + <kbd>S</kbd> - Save</p>
        <p><kbd>Escape</kbd> - Cancel</p>
      </div>
    </div>
  );
}
```

**Event Listener Patterns:**

1. **Window events:**
```tsx
useEffect(() => {
  const handleScroll = () => {
    console.log('Scrolled');
  };
  
  window.addEventListener('scroll', handleScroll);
  return () => window.removeEventListener('scroll', handleScroll);
}, []);
```

2. **Document events:**
```tsx
useEffect(() => {
  const handleClick = (e) => {
    console.log('Document clicked');
  };
  
  document.addEventListener('click', handleClick);
  return () => document.removeEventListener('click', handleClick);
}, []);
```

3. **Custom element events:**
```tsx
useEffect(() => {
  const element = document.getElementById('my-element');
  const handleCustomEvent = (e) => {
    console.log('Custom event:', e.detail);
  };
  
  element?.addEventListener('custom-event', handleCustomEvent);
  return () => {
    element?.removeEventListener('custom-event', handleCustomEvent);
  };
}, []);
```

**Common Mistakes:**

1. **❌ Not removing listener:**
```tsx
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // No cleanup - memory leak!
}, []);
```

2. **✅ Correct: Remove listener:**
```tsx
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

3. **❌ Function recreated on every render:**
```tsx
useEffect(() => {
  const handleResize = () => { ... }; // New function every render
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []); // Can't remove properly
```

4. **✅ Correct: Use useCallback or define outside:**
```tsx
const handleResize = useCallback(() => {
  // ... handle resize
}, []);

useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, [handleResize]);
```

**Best Practices:**
- Always cleanup event listeners
- Use useCallback for handlers
- Prevent default when needed
- Use passive listeners for scroll
- Consider performance impact

---

### 11. Common mistakes

**What is it?**
Understanding and avoiding the most common mistakes developers make when using useEffect.

**Why we need it?**
- Prevent bugs and errors
- Write better code
- Improve performance
- Avoid memory leaks
- Follow best practices

**How it works?**
- Learn from common errors
- Understand the pitfalls
- Know the correct patterns
- Apply best practices
- Use ESLint rules

**Common Mistakes:**

1. **Missing Dependencies:**
```tsx
// ❌ Wrong
useEffect(() => {
  console.log(count);
}, []);

// ✅ Correct
useEffect(() => {
  console.log(count);
}, [count]);
```

2. **Infinite Loops:**
```tsx
// ❌ Wrong - sets state that's in dependencies
useEffect(() => {
  setData([...data, newItem]);
}, [data]);

// ✅ Correct - use functional update
useEffect(() => {
  setData(prev => [...prev, newItem]);
}, []);
```

3. **Not Cleaning Up:**
```tsx
// ❌ Wrong
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
}, []);

// ✅ Correct
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
  return () => clearInterval(interval);
}, []);
```

4. **Async Effect Function:**
```tsx
// ❌ Wrong
useEffect(async () => {
  const data = await fetch('/api/data');
  setData(data);
}, []);

// ✅ Correct
useEffect(() => {
  const fetchData = async () => {
    const data = await fetch('/api/data');
    setData(data);
  };
  fetchData();
}, []);
```

5. **Doing Work During Render:**
```tsx
// ❌ Wrong
function Component() {
  document.title = `Count: ${count}`; // Side effect during render
  return <div>{count}</div>;
}

// ✅ Correct
function Component() {
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);
  return <div>{count}</div>;
}
```

6. **Stale Closures:**
```tsx
// ❌ Wrong
useEffect(() => {
  const interval = setInterval(() => {
    console.log(count); // Stale count
  }, 1000);
  return () => clearInterval(interval);
}, []);

// ✅ Correct
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => {
      console.log(prev); // Latest count
      return prev + 1;
    });
  }, 1000);
  return () => clearInterval(interval);
}, []);
```

7. **Multiple Effects Depending on Each Other:**
```tsx
// ❌ Wrong
useEffect(() => {
  setData1(newValue);
}, []);

useEffect(() => {
  setData2(data1 + 1); // Might run before data1 updates
}, [data1]);

// ✅ Correct
useEffect(() => {
  const newValue = compute();
  setData1(newValue);
  setData2(newValue + 1);
}, []);
```

8. **Overusing useEffect:**
```tsx
// ❌ Wrong - useEffect for derived state
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ Correct - derived state
const fullName = `${firstName} ${lastName}`;
```

**Best Practices to Avoid Mistakes:**
- Use ESLint with React hooks plugin
- Include all dependencies
- Always cleanup when needed
- Don't make effect function async
- Use derived state when possible
- Keep effects focused

---

### 12. Infinite loops

**What is it?**
A common bug where useEffect triggers itself repeatedly, causing the component to render in an infinite loop.

**Why we need to avoid it?**
- Crashes the application
- Freezes the browser
- Consumes resources
- Poor user experience
- Difficult to debug

**How it happens?**
- Effect updates state
- State is in dependencies
- Effect re-runs
- Updates state again
- Cycle continues

**Common Causes:**

1. **State update in dependencies:**
```tsx
// ❌ Infinite loop
useEffect(() => {
  setData([...data, newItem]);
}, [data]); // data updates → effect runs → data updates...
```

2. **Object reference changes:**
```tsx
// ❌ Infinite loop
const config = { value: 'test' }; // New reference every render

useEffect(() => {
  console.log(config);
}, [config]); // Re-runs every render
```

3. **Callback function changes:**
```tsx
// ❌ Infinite loop
const handleClick = () => {
  // New function every render
};

useEffect(() => {
  element.addEventListener('click', handleClick);
  return () => element.removeEventListener('click', handleClick);
}, [handleClick]); // Re-runs every render
```

**Solutions:**

1. **Use functional updates:**
```tsx
// ✅ Correct
useEffect(() => {
  setData(prev => [...prev, newItem]);
}, []); // No dependency on data
```

2. **Use useMemo for objects:**
```tsx
// ✅ Correct
const config = useMemo(() => ({ value: 'test' }), []);

useEffect(() => {
  console.log(config);
}, [config]); // Stable reference
```

3. **Use useCallback for functions:**
```tsx
// ✅ Correct
const handleClick = useCallback(() => {
  // ... handle click
}, []);

useEffect(() => {
  element.addEventListener('click', handleClick);
  return () => element.removeEventListener('click', handleClick);
}, [handleClick]); // Stable function
```

4. **Remove unnecessary dependencies:**
```tsx
// ✅ Correct
useEffect(() => {
  // If you don't need to re-run, don't include dependency
  console.log('One-time setup');
}, []);
```

**Debugging Infinite Loops:**

1. **Add console logs:**
```tsx
useEffect(() => {
  console.log('Effect ran');
  setData(newValue);
}, [data]);
```

2. **Check dependencies:**
```tsx
// Does the effect update any of its dependencies?
useEffect(() => {
  // Updates data which is in dependencies
  setData([...data, item]);
}, [data]); // ← Problem here
```

3. **Use React DevTools:**
- Highlight updates
- Check component re-renders
- Inspect hooks

**Best Practices:**
- Use functional updates when possible
- Stabilize object/function references
- Only include necessary dependencies
- Check for state updates in effects
- Use ESLint to detect issues

---

### 13. Missing dependencies

**What is it?**
When useEffect uses values that aren't included in the dependency array, causing stale closures and unexpected behavior.

**Why we need to avoid it?**
- Stale data in effects
- Bugs from old values
- Unpredictable behavior
- Hard to debug
- ESLint warnings

**How it happens?**
- Effect uses value
- Value not in dependencies
- Effect has stale closure
- Changes to value ignored
- Effect uses old value

**Examples:**

1. **Missing state dependency:**
```tsx
// ❌ Wrong - count not in dependencies
useEffect(() => {
  console.log(count); // Always shows initial count
}, []);

// ✅ Correct
useEffect(() => {
  console.log(count);
}, [count]);
```

2. **Missing prop dependency:**
```tsx
// ❌ Wrong - userId not in dependencies
useEffect(() => {
  fetch(`/api/users/${userId}`).then(setUser);
}, []);

// ✅ Correct
useEffect(() => {
  fetch(`/api/users/${userId}`).then(setUser);
}, [userId]);
```

3. **Missing function dependency:**
```tsx
// ❌ Wrong - fetchData not in dependencies
useEffect(() => {
  fetchData();
}, []);

// ✅ Correct
useEffect(() => {
  fetchData();
}, [fetchData]);
```

**Consequences of Missing Dependencies:**

1. **Stale Closures:**
```tsx
const [count, setCount] = useState(0);

useEffect(() => {
  const interval = setInterval(() => {
    console.log(count); // Always 0 - stale closure
  }, 1000);
  return () => clearInterval(interval);
}, []); // count missing
```

2. **Outdated Data:**
```tsx
function UserList({ userId }: { userId: number }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`/api/users/${userId}`).then(setUser);
  }, []); // userId missing - always fetches initial user

  return <div>{user?.name}</div>;
}
```

3. **Lost Updates:**
```tsx
useEffect(() => {
  console.log(settings.theme); // Always initial theme
}, []); // settings missing
```

**Solutions:**

1. **Include all dependencies:**
```tsx
useEffect(() => {
  console.log(count);
}, [count]); // Include count
```

2. **Use functional updates:**
```tsx
useEffect(() => {
  setCount(prev => prev + 1); // No need for count in deps
}, []);
```

3. **Extract stable values:**
```tsx
const stableValue = useMemo(() => compute(value), [value]);

useEffect(() => {
  console.log(stableValue);
}, [stableValue]);
```

4. **Disable ESLint rule (with caution):**
```tsx
useEffect(() => {
  console.log(count);
  // eslint-disable-next-line react-hooks/exhaustive-deps
}, []); // Only if you're sure
```

**Best Practices:**
- Always include dependencies
- Use ESLint to detect issues
- Understand why each dependency is needed
- Use functional updates when appropriate
- Document intentional exclusions

---

### 14. Unnecessary useEffect

**What is it?**
Using useEffect when it's not needed, often for operations that should happen during rendering or for derived state.

**Why we need to avoid it?**
- Unnecessary complexity
- Performance overhead
- Harder to understand
- Potential bugs
- More code to maintain

**Common Unnecessary useEffect Patterns:**

1. **Derived State:**
```tsx
// ❌ Wrong - useEffect for derived state
const [items, setItems] = useState([]);
const [filteredItems, setFilteredItems] = useState([]);

useEffect(() => {
  setFilteredItems(items.filter(item => item.active));
}, [items]);

// ✅ Correct - derived state
const [items, setItems] = useState([]);
const filteredItems = items.filter(item => item.active);
```

2. **Data Transformation:**
```tsx
// ❌ Wrong - useEffect for transformation
const [rawData, setRawData] = useState([]);
const [processedData, setProcessedData] = useState([]);

useEffect(() => {
  setProcessedData(rawData.map(item => ({
    ...item,
    formatted: new Date(item.date).toLocaleDateString()
  })));
}, [rawData]);

// ✅ Correct - useMemo for expensive computation
const [rawData, setRawData] = useState([]);
const processedData = useMemo(() => 
  rawData.map(item => ({
    ...item,
    formatted: new Date(item.date).toLocaleDateString()
  })),
  [rawData]
);
```

3. **Prop to State Sync:**
```tsx
// ❌ Wrong - useEffect to sync prop to state
function Component({ value }: { value: string }) {
  const [localValue, setLocalValue] = useState(value);

  useEffect(() => {
    setLocalValue(value);
  }, [value]);

  return <input value={localValue} onChange={e => setLocalValue(e.target.value)} />;
}

// ✅ Correct - use key or controlled component
function Component({ value }: { value: string }) {
  return <input value={value} onChange={onChange} />;
}

// Or use key to reset
function Component({ value }: { value: string }) {
  const [localValue, setLocalValue] = useState(value);
  return <input key={value} value={localValue} onChange={e => setLocalValue(e.target.value)} />;
}
```

4. **Simple Calculations:**
```tsx
// ❌ Wrong - useEffect for simple calculation
const [a, setA] = useState(0);
const [b, setB] = useState(0);
const [sum, setSum] = useState(0);

useEffect(() => {
  setSum(a + b);
}, [a, b]);

// ✅ Correct - derived state
const [a, setA] = useState(0);
const [b, setB] = useState(0);
const sum = a + b;
```

**When useEffect IS Needed:**

1. **Data fetching:**
```tsx
useEffect(() => {
  fetch('/api/data').then(setData);
}, []);
```

2. **Subscriptions:**
```tsx
useEffect(() => {
  const subscription = api.subscribe();
  return () => subscription.unsubscribe();
}, []);
```

3. **DOM manipulation:**
```tsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

4. **Browser APIs:**
```tsx
useEffect(() => {
  navigator.geolocation.getCurrentPosition(setPosition);
}, []);
```

**Best Practices:**
- Use derived state for calculations
- Use useMemo for expensive computations
- Only use useEffect for side effects
- Keep effects focused
- Question every useEffect

---

### 15. When NOT to use useEffect

**What is it?**
Understanding scenarios where useEffect is not the right tool and alternative approaches should be used instead.

**Why we need to know?**
- Write simpler code
- Better performance
- Fewer bugs
- Clearer intent
- Follow React patterns

**When NOT to use useEffect:**

1. **For Derived State:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ Use derived state
const fullName = `${firstName} ${lastName}`;
```

2. **For Props to State Sync:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  setLocalValue(value);
}, [value]);

// ✅ Use key or controlled component
<input key={value} value={value} onChange={onChange} />
```

3. **For Data Transformation:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  setFiltered(data.filter(item => item.active));
}, [data]);

// ✅ Use derived state or useMemo
const filtered = data.filter(item => item.active);
// or
const filtered = useMemo(() => data.filter(item => item.active), [data]);
```

4. **For Simple Calculations:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  setTotal(price * quantity);
}, [price, quantity]);

// ✅ Use derived state
const total = price * quantity;
```

5. **For Updating State Based on Other State:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  if (isValid) {
    setError(null);
  }
}, [isValid]);

// ✅ Update during render or in event handler
const handleSubmit = () => {
  if (isValid) {
    setError(null);
  }
};
```

6. **For Formatting Display Values:**
```tsx
// ❌ Don't use useEffect
useEffect(() => {
  setFormattedDate(new Date(date).toLocaleDateString());
}, [date]);

// ✅ Format during render
<p>{new Date(date).toLocaleDateString()}</p>
```

**When TO use useEffect:**

1. **Data Fetching:**
```tsx
useEffect(() => {
  fetch('/api/data').then(setData);
}, [id]);
```

2. **Subscriptions:**
```tsx
useEffect(() => {
  const ws = new WebSocket(url);
  return () => ws.close();
}, [url]);
```

3. **DOM Manipulation:**
```tsx
useEffect(() => {
  document.title = title;
}, [title]);
```

4. **Browser APIs:**
```tsx
useEffect(() => {
  navigator.geolocation.watchPosition(setPosition);
}, []);
```

5. **Timers:**
```tsx
useEffect(() => {
  const timer = setTimeout(() => {}, 1000);
  return () => clearTimeout(timer);
}, []);
```

6. **Event Listeners:**
```tsx
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

**Decision Flowchart:**

```
Is this a side effect?
├─ Yes → Use useEffect
└─ No → Don't use useEffect
    ├─ Is it derived state?
    │   └─ Yes → Calculate during render
    ├─ Is it data transformation?
    │   └─ Yes → Use useMemo (if expensive) or render
    ├─ Is it props to state sync?
    │   └─ Yes → Use key or controlled component
    └─ Is it a simple calculation?
        └─ Yes → Calculate during render
```

**Best Practices:**
- Only use useEffect for side effects
- Calculate derived state during render
- Use useMemo for expensive computations
- Question every useEffect
- Keep components simple

---

## Practical Project: Login + Register Form

Let's build a complete authentication system with Login and Register forms that demonstrates all the concepts we've learned in this session.

### Project Overview

We'll create:
- **Login Form:** Email, password, remember me, validation, loading, error, success states
- **Register Form:** Name, email, password, confirm password, validation
- **Auth State Management:** Using useState
- **Persistence:** Using useEffect with localStorage
- **Form Validation:** Real-time and on-submit
- **Loading & Error States:** Proper UI feedback

### Step 1: Project Setup

```bash
# Create new project
npm create vite@latest auth-app -- --template react-ts

# Navigate to project
cd auth-app

# Install dependencies
npm install

# Start development server
npm run dev
```

### Step 2: Define Types

Create `src/types/index.ts`:

```typescript
// src/types/index.ts
export interface LoginFormData {
  email: string;
  password: string;
  rememberMe: boolean;
}

export interface RegisterFormData {
  name: string;
  email: string;
  password: string;
  confirmPassword: string;
}

export interface AuthError {
  field?: string;
  message: string;
}

export interface User {
  id: string;
  name: string;
  email: string;
}
```

### Step 3: Create Validation Utilities

Create `src/utils/validation.ts`:

```typescript
// src/utils/validation.ts

export const validateEmail = (email: string): string | null => {
  if (!email.trim()) {
    return 'Email is required';
  }
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    return 'Please enter a valid email address';
  }
  return null;
};

export const validatePassword = (password: string): string | null => {
  if (!password) {
    return 'Password is required';
  }
  if (password.length < 8) {
    return 'Password must be at least 8 characters';
  }
  if (!/[A-Z]/.test(password)) {
    return 'Password must contain at least one uppercase letter';
  }
  if (!/[a-z]/.test(password)) {
    return 'Password must contain at least one lowercase letter';
  }
  if (!/[0-9]/.test(password)) {
    return 'Password must contain at least one number';
  }
  return null;
};

export const validateName = (name: string): string | null => {
  if (!name.trim()) {
    return 'Name is required';
  }
  if (name.trim().length < 2) {
    return 'Name must be at least 2 characters';
  }
  return null;
};

export const validateConfirmPassword = (password: string, confirmPassword: string): string | null => {
  if (!confirmPassword) {
    return 'Please confirm your password';
  }
  if (password !== confirmPassword) {
    return 'Passwords do not match';
  }
  return null;
};
```

### Step 4: Create Login Form Component

Create `src/components/LoginForm.tsx`:

```typescript
// src/components/LoginForm.tsx
import { useState, useEffect } from 'react';
import { LoginFormData, AuthError, User } from '../types';
import { validateEmail, validatePassword } from '../utils/validation';

interface LoginFormProps {
  onLogin: (user: User) => void;
  onSwitchToRegister: () => void;
}

function LoginForm({ onLogin, onSwitchToRegister }: LoginFormProps) {
  const [formData, setFormData] = useState<LoginFormData>({
    email: '',
    password: '',
    rememberMe: false
  });

  const [errors, setErrors] = useState<Record<string, string>>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});
  const [isLoading, setIsLoading] = useState(false);
  const [apiError, setApiError] = useState<string | null>(null);

  // Load saved email if remember me was checked
  useEffect(() => {
    const savedEmail = localStorage.getItem('rememberedEmail');
    if (savedEmail) {
      setFormData(prev => ({ ...prev, email: savedEmail, rememberMe: true }));
    }
  }, []);

  const validateField = (field: string, value: string): string | null => {
    switch (field) {
      case 'email':
        return validateEmail(value);
      case 'password':
        return validatePassword(value);
      default:
        return null;
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value, type, checked } = e.target;
    const newValue = type === 'checkbox' ? checked : value;
    
    setFormData(prev => ({ ...prev, [name]: newValue }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
    
    // Clear API error when user modifies form
    if (apiError) {
      setApiError(null);
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name, value);
    if (error) {
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};
    let isValid = true;

    const emailError = validateEmail(formData.email);
    if (emailError) {
      newErrors.email = emailError;
      isValid = false;
    }

    const passwordError = validatePassword(formData.password);
    if (passwordError) {
      newErrors.password = passwordError;
      isValid = false;
    }

    setErrors(newErrors);
    setTouched({
      email: true,
      password: true
    });

    return isValid;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) {
      return;
    }

    setIsLoading(true);
    setApiError(null);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));

      // Simulate login validation
      if (formData.email === 'test@example.com' && formData.password === 'Password123') {
        const user: User = {
          id: '1',
          name: 'Test User',
          email: formData.email
        };

        // Save email if remember me is checked
        if (formData.rememberMe) {
          localStorage.setItem('rememberedEmail', formData.email);
        } else {
          localStorage.removeItem('rememberedEmail');
        }

        onLogin(user);
      } else {
        setApiError('Invalid email or password');
      }
    } catch (error) {
      setApiError('An error occurred. Please try again.');
      console.error('Login error:', error);
    } finally {
      setIsLoading(false);
    }
  };

  const isFormValid = Object.keys(errors).length === 0 &&
    formData.email.trim() !== '' &&
    formData.password.trim() !== '';

  return (
    <div className="auth-form-container">
      <div className="auth-form">
        <h2>Login</h2>
        <p>Welcome back! Please login to your account.</p>

        {apiError && (
          <div className="error-banner">
            <span className="error-icon">⚠️</span>
            {apiError}
          </div>
        )}

        <form onSubmit={handleSubmit}>
          <div className="form-group">
            <label htmlFor="login-email">Email</label>
            <input
              type="email"
              id="login-email"
              name="email"
              value={formData.email}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Enter your email"
              className={errors.email ? 'error' : ''}
              disabled={isLoading}
            />
            {errors.email && <span className="error-message">{errors.email}</span>}
          </div>

          <div className="form-group">
            <label htmlFor="login-password">Password</label>
            <input
              type="password"
              id="login-password"
              name="password"
              value={formData.password}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Enter your password"
              className={errors.password ? 'error' : ''}
              disabled={isLoading}
            />
            {errors.password && <span className="error-message">{errors.password}</span>}
          </div>

          <div className="form-group checkbox-group">
            <label className="checkbox-label">
              <input
                type="checkbox"
                name="rememberMe"
                checked={formData.rememberMe}
                onChange={handleChange}
                disabled={isLoading}
              />
              Remember me
            </label>
            <a href="#" className="forgot-password">Forgot password?</a>
          </div>

          <button type="submit" className="submit-button" disabled={!isFormValid || isLoading}>
            {isLoading ? 'Logging in...' : 'Login'}
          </button>
        </form>

        <div className="form-footer">
          <p>Don't have an account? <button onClick={onSwitchToRegister} className="link-button">Register</button></p>
        </div>
      </div>
    </div>
  );
}

export default LoginForm;
```

### Step 5: Create Register Form Component

Create `src/components/RegisterForm.tsx`:

```typescript
// src/components/RegisterForm.tsx
import { useState } from 'react';
import { RegisterFormData, User } from '../types';
import { validateEmail, validatePassword, validateName, validateConfirmPassword } from '../utils/validation';

interface RegisterFormProps {
  onRegister: (user: User) => void;
  onSwitchToLogin: () => void;
}

function RegisterForm({ onRegister, onSwitchToLogin }: RegisterFormProps) {
  const [formData, setFormData] = useState<RegisterFormData>({
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
  });

  const [errors, setErrors] = useState<Record<string, string>>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});
  const [isLoading, setIsLoading] = useState(false);
  const [apiError, setApiError] = useState<string | null>(null);
  const [success, setSuccess] = useState(false);

  const validateField = (field: string, value: string): string | null => {
    switch (field) {
      case 'name':
        return validateName(value);
      case 'email':
        return validateEmail(value);
      case 'password':
        return validatePassword(value);
      case 'confirmPassword':
        return validateConfirmPassword(formData.password, value);
      default:
        return null;
    }
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: '' }));
    }
    
    // Clear API error when user modifies form
    if (apiError) {
      setApiError(null);
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    
    const error = validateField(name, value);
    if (error) {
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};
    let isValid = true;

    const nameError = validateName(formData.name);
    if (nameError) {
      newErrors.name = nameError;
      isValid = false;
    }

    const emailError = validateEmail(formData.email);
    if (emailError) {
      newErrors.email = emailError;
      isValid = false;
    }

    const passwordError = validatePassword(formData.password);
    if (passwordError) {
      newErrors.password = passwordError;
      isValid = false;
    }

    const confirmPasswordError = validateConfirmPassword(formData.password, formData.confirmPassword);
    if (confirmPasswordError) {
      newErrors.confirmPassword = confirmPasswordError;
      isValid = false;
    }

    setErrors(newErrors);
    setTouched({
      name: true,
      email: true,
      password: true,
      confirmPassword: true
    });

    return isValid;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    if (!validateForm()) {
      return;
    }

    setIsLoading(true);
    setApiError(null);

    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));

      // Simulate registration
      const user: User = {
        id: Date.now().toString(),
        name: formData.name,
        email: formData.email
      };

      setSuccess(true);
      
      setTimeout(() => {
        onRegister(user);
      }, 1500);

    } catch (error) {
      setApiError('Registration failed. Please try again.');
      console.error('Registration error:', error);
    } finally {
      setIsLoading(false);
    }
  };

  const isFormValid = Object.keys(errors).length === 0 &&
    formData.name.trim() !== '' &&
    formData.email.trim() !== '' &&
    formData.password.trim() !== '' &&
    formData.confirmPassword.trim() !== '';

  return (
    <div className="auth-form-container">
      <div className="auth-form">
        <h2>Create Account</h2>
        <p>Join us today! Create your account to get started.</p>

        {apiError && (
          <div className="error-banner">
            <span className="error-icon">⚠️</span>
            {apiError}
          </div>
        )}

        {success && (
          <div className="success-banner">
            <span className="success-icon">✓</span>
            Registration successful! Redirecting...
          </div>
        )}

        <form onSubmit={handleSubmit}>
          <div className="form-group">
            <label htmlFor="register-name">Full Name</label>
            <input
              type="text"
              id="register-name"
              name="name"
              value={formData.name}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Enter your full name"
              className={errors.name ? 'error' : ''}
              disabled={isLoading || success}
            />
            {errors.name && <span className="error-message">{errors.name}</span>}
          </div>

          <div className="form-group">
            <label htmlFor="register-email">Email</label>
            <input
              type="email"
              id="register-email"
              name="email"
              value={formData.email}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Enter your email"
              className={errors.email ? 'error' : ''}
              disabled={isLoading || success}
            />
            {errors.email && <span className="error-message">{errors.email}</span>}
          </div>

          <div className="form-group">
            <label htmlFor="register-password">Password</label>
            <input
              type="password"
              id="register-password"
              name="password"
              value={formData.password}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Create a password"
              className={errors.password ? 'error' : ''}
              disabled={isLoading || success}
            />
            {errors.password && <span className="error-message">{errors.password}</span>}
            <div className="password-requirements">
              <p>Password must contain:</p>
              <ul>
                <li>At least 8 characters</li>
                <li>One uppercase letter</li>
                <li>One lowercase letter</li>
                <li>One number</li>
              </ul>
            </div>
          </div>

          <div className="form-group">
            <label htmlFor="register-confirm-password">Confirm Password</label>
            <input
              type="password"
              id="register-confirm-password"
              name="confirmPassword"
              value={formData.confirmPassword}
              onChange={handleChange}
              onBlur={handleBlur}
              placeholder="Confirm your password"
              className={errors.confirmPassword ? 'error' : ''}
              disabled={isLoading || success}
            />
            {errors.confirmPassword && <span className="error-message">{errors.confirmPassword}</span>}
          </div>

          <button type="submit" className="submit-button" disabled={!isFormValid || isLoading || success}>
            {isLoading ? 'Creating account...' : success ? 'Account created!' : 'Create Account'}
          </button>
        </form>

        <div className="form-footer">
          <p>Already have an account? <button onClick={onSwitchToLogin} className="link-button">Login</button></p>
        </div>
      </div>
    </div>
  );
}

export default RegisterForm;
```

### Step 6: Create Main App Component

Create `src/App.tsx`:

```typescript
// src/App.tsx
import { useState, useEffect } from 'react';
import LoginForm from './components/LoginForm';
import RegisterForm from './components/RegisterForm';
import { User } from './types';

function App() {
  const [isLoginView, setIsLoginView] = useState(true);
  const [user, setUser] = useState<User | null>(null);

  // Check for existing session on mount
  useEffect(() => {
    const savedUser = localStorage.getItem('currentUser');
    if (savedUser) {
      setUser(JSON.parse(savedUser));
    }
  }, []);

  const handleLogin = (userData: User) => {
    setUser(userData);
    localStorage.setItem('currentUser', JSON.stringify(userData));
  };

  const handleRegister = (userData: User) => {
    setUser(userData);
    localStorage.setItem('currentUser', JSON.stringify(userData));
  };

  const handleLogout = () => {
    setUser(null);
    localStorage.removeItem('currentUser');
  };

  if (user) {
    return (
      <div className="app">
        <div className="dashboard">
          <h1>Welcome, {user.name}!</h1>
          <p>You are logged in as {user.email}</p>
          <button onClick={handleLogout} className="logout-button">
            Logout
          </button>
        </div>
      </div>
    );
  }

  return (
    <div className="app">
      <div className="auth-container">
        {isLoginView ? (
          <LoginForm
            onLogin={handleLogin}
            onSwitchToRegister={() => setIsLoginView(false)}
          />
        ) : (
          <RegisterForm
            onRegister={handleRegister}
            onSwitchToLogin={() => setIsLoginView(true)}
          />
        )}
      </div>
    </div>
  );
}

export default App;
```

### Step 7: Add Styling

Add to `src/index.css`:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.app {
  width: 100%;
  max-width: 1200px;
  padding: 20px;
}

.auth-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.auth-form-container {
  width: 100%;
  max-width: 450px;
}

.auth-form {
  background: white;
  border-radius: 12px;
  padding: 40px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
}

.auth-form h2 {
  font-size: 28px;
  margin-bottom: 8px;
  color: #333;
}

.auth-form > p {
  color: #666;
  margin-bottom: 32px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.form-group input:focus {
  outline: none;
  border-color: #667eea;
}

.form-group input.error {
  border-color: #e74c3c;
}

.form-group input:disabled {
  background-color: #f5f5f5;
  cursor: not-allowed;
}

.error-message {
  display: block;
  margin-top: 6px;
  color: #e74c3c;
  font-size: 14px;
}

.checkbox-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.checkbox-label input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.forgot-password {
  color: #667eea;
  text-decoration: none;
  font-size: 14px;
}

.forgot-password:hover {
  text-decoration: underline;
}

.password-requirements {
  margin-top: 8px;
  padding: 12px;
  background: #f8f9fa;
  border-radius: 6px;
  font-size: 13px;
  color: #666;
}

.password-requirements p {
  font-weight: 500;
  margin-bottom: 4px;
}

.password-requirements ul {
  list-style-position: inside;
  margin: 0;
}

.submit-button {
  width: 100%;
  padding: 14px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.submit-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.submit-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.form-footer {
  margin-top: 24px;
  text-align: center;
  color: #666;
}

.link-button {
  background: none;
  border: none;
  color: #667eea;
  font-weight: 600;
  cursor: pointer;
  padding: 0;
  font-size: inherit;
}

.link-button:hover {
  text-decoration: underline;
}

.error-banner {
  background: #fee;
  border: 1px solid #fcc;
  border-radius: 8px;
  padding: 12px 16px;
  margin-bottom: 24px;
  display: flex;
  align-items: center;
  gap: 8px;
  color: #c33;
}

.error-icon {
  font-size: 18px;
}

.success-banner {
  background: #efe;
  border: 1px solid #cfc;
  border-radius: 8px;
  padding: 12px 16px;
  margin-bottom: 24px;
  display: flex;
  align-items: center;
  gap: 8px;
  color: #363;
}

.success-icon {
  font-size: 18px;
  font-weight: bold;
}

.dashboard {
  background: white;
  border-radius: 12px;
  padding: 40px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.dashboard h1 {
  font-size: 32px;
  margin-bottom: 16px;
  color: #333;
}

.dashboard p {
  color: #666;
  margin-bottom: 24px;
}

.logout-button {
  padding: 12px 24px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s;
}

.logout-button:hover {
  background: #c0392b;
}

@media (max-width: 768px) {
  .auth-form {
    padding: 24px;
  }

  .auth-form h2 {
    font-size: 24px;
  }
}
```

### Step 8: Key Concepts Demonstrated

**Forms:**
- Controlled components for all inputs
- Multiple input types (text, email, password, checkbox)
- Form state management with useState
- Real-time validation on blur
- Form validation on submit
- Error message display
- Loading state during submission
- Success feedback

**useEffect:**
- Loading saved email on mount (empty dependency array)
- Checking for existing session on mount
- localStorage operations
- Cleanup if needed (not shown in this simple example)

**State Management:**
- Form state (object with multiple fields)
- Validation state (errors, touched)
- UI state (loading, error, success)
- Auth state (user, isLoginView)

### Step 9: Testing the Application

```bash
# Make sure the dev server is running
npm run dev
```

**Test Login:**
- Email: `test@example.com`
- Password: `Password123`
- Try "Remember me" functionality

**Test Register:**
- Create a new account
- Validate password requirements
- Test password confirmation

### Step 10: Exercise for Students

Try these modifications to reinforce your understanding:

1. **Add "Show Password" toggle** to reveal/hide password
2. **Implement password strength indicator** (weak, medium, strong)
3. **Add form reset functionality** to clear all fields
4. **Implement debounced validation** instead of blur validation
5. **Add "Forgot Password" flow** with email verification

---

## Comprehensive Review

### Session Summary

In this session, we covered:

1. **Forms:** HTML vs React forms, controlled components, input types (text, number, checkbox, radio, select, textarea), form submission, validation, error handling, and form reset
2. **Rendering vs Event vs Side Effect:** Understanding the fundamental differences and when to use each approach
3. **useEffect:** Syntax, dependencies, cleanup, common mistakes, infinite loops, and when NOT to use useEffect
4. **Practical Project:** Built a complete Login + Register form with validation, loading states, and localStorage persistence

### Key Takeaways

- Use controlled components for forms in React
- Always prevent default form submission with `e.preventDefault()`
- Implement validation both on blur and on submit
- Provide clear error messages and user feedback
- useEffect is for side effects, not for derived state
- Always include dependencies in useEffect array
- Use cleanup functions to prevent memory leaks
- Avoid infinite loops by not updating state that's in dependencies
- Use functional updates in setInterval to avoid stale closures
- Derived state should be calculated during render, not in useEffect

---

## 15 Student Questions

1. What is the difference between controlled and uncontrolled components in React?
2. Why should you use `e.preventDefault()` in React form handlers?
3. When should you use functional updates instead of direct updates in useEffect?
4. What is the purpose of the dependency array in useEffect?
5. How do you handle form validation in React?
6. What is a side effect in React?
7. Why is cleanup important in useEffect?
8. What causes infinite loops in useEffect?
9. How do you prevent stale closures in setInterval?
10. When should you NOT use useEffect?
11. What is the difference between rendering, events, and side effects?
12. How do you implement "remember me" functionality?
13. What is derived state and how is it different from state?
14. How do you handle async operations in useEffect?
15. What are the common mistakes when using useEffect?

---

## 5 Interview Questions

### 1. Explain the difference between controlled and uncontrolled components, and when to use each.

**Answer:** Controlled components have their value controlled by React state, with onChange handlers updating state. Uncontrolled components maintain their own internal state, accessed via refs.

Use controlled components when:
- Need real-time validation
- Want consistent state management
- Need to control form behavior
- Building complex forms

Use uncontrolled components when:
- Simple forms with minimal validation
- Integrating with non-React code
- Performance-critical large forms
- File uploads

### 2. How does useEffect handle cleanup, and why is it important?

**Answer:** useEffect cleanup is handled by returning a function from the effect. This cleanup function runs before the component unmounts and before the effect re-runs (if dependencies change).

Cleanup is important because:
- Prevents memory leaks
- Removes event listeners
- Clears timers and intervals
- Closes connections and subscriptions
- Resets external state

Example:
```tsx
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
  return () => clearInterval(interval); // Cleanup
}, []);
```

### 3. What are the common causes of infinite loops in useEffect, and how do you prevent them?

**Answer:** Common causes:
- Updating state that's in the dependency array
- Object references changing on every render
- Function references changing on every render

Prevention:
- Use functional updates when possible
- Use useMemo for objects
- Use useCallback for functions
- Only include necessary dependencies
- Use ESLint to detect issues

### 4. Explain the concept of derived state and when to use it instead of useEffect.

**Answer:** Derived state is calculated from other state or props during rendering, rather than being stored separately.

Use derived state when:
- Data can be calculated from existing state
- No need to store duplicate data
- Calculation is simple/fast
- Want to avoid state synchronization issues

Don't use useEffect for derived state. Calculate during render instead.

Example:
```tsx
// ❌ Wrong
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ Correct
const fullName = `${firstName} ${lastName}`;
```

### 5. What is the difference between rendering, events, and side effects in React?

**Answer:**
- **Rendering:** Displaying UI based on state/props. Happens automatically when state/props change. Pure and predictable.
- **Events:** User interactions (click, change, submit). Triggered by user actions. Handled by event handlers.
- **Side Effects:** Operations outside rendering (API calls, DOM manipulation). Handled by useEffect. Run after render.

Understanding these differences is crucial for knowing when to use useState for state/events and when to use useEffect for side effects.

---

## 5 Practical Exercises

### Exercise 1: Contact Form with Advanced Validation

Create a contact form with:
- Name, email, subject, message fields
- Real-time validation on input
- Validation on blur
- Form validation on submit
- Loading state during submission
- Success/error feedback
- Form reset after successful submission

**Requirements:**
- Use controlled components
- Implement all validation rules
- Show appropriate error messages
- Handle loading and error states

### Exercise 2: Data Fetching with useEffect

Create a component that:
- Fetches data from an API on mount
- Shows loading state
- Handles errors
- Refetches when a filter changes
- Cleans up fetch on unmount
- Shows data in a list

**Requirements:**
- Use useEffect with proper dependencies
- Implement AbortController for cleanup
- Handle all error states
- Show loading indicator

### Exercise 3: Timer with Start/Stop/Reset

Create a timer component with:
- Start, stop, and reset functionality
- Display time in MM:SS format
- Use setInterval with cleanup
- Persist timer state in localStorage
- Resume timer on page reload

**Requirements:**
- Use functional updates in setInterval
- Proper cleanup of interval
- localStorage persistence with useEffect
- Handle component unmount

### Exercise 4: Window Size Tracker

Create a component that:
- Tracks window width and height
- Updates on window resize
- Debounces resize events
- Shows current dimensions
- Cleans up event listener on unmount

**Requirements:**
- Add event listener in useEffect
- Implement debounce function
- Proper cleanup of event listener
- Use useCallback for handler

### Exercise 5: Search with Debouncing

Create a search component that:
- Has a search input
- Debounces search queries
- Fetches results after debounce
- Shows loading state
- Displays search results
- Cancels pending requests on new search

**Requirements:**
- Implement debounce with useEffect
- Use AbortController for fetch cancellation
- Handle loading and error states
- Show results in a list

---

## Homework

### Reading Assignment
1. Read the official React documentation on "Forms"
2. Read the React documentation on "Hooks API Reference - useEffect"
3. Read the React documentation on "You Might Not Need an Effect"

### Practice Exercises
1. **Form Enhancement:** Take the Login form we built and add:
   - Show/hide password toggle
   - Password strength indicator
   - Social login buttons
   - "Forgot Password" link with flow

2. **useEffect Patterns:** Create components demonstrating:
   - Data fetching with dependencies
   - Event listeners with cleanup
   - setInterval with functional updates
   - localStorage synchronization
   - Document title updates

3. **Validation System:** Create a reusable validation system:
   - Validation rules interface
   - Generic validator function
   - Error message mapping
   - Real-time validation hook
   - Form validation hook

### Research Project
Research and write a brief comparison (500 words) of different form libraries in React:
- React Hook Form
- Formik
- Final Form
- Unform

Include pros and cons of each and recommend use cases for each.

---

## Challenge Exercise

### Advanced Dashboard with Forms and Effects

Build a comprehensive dashboard that demonstrates advanced form handling and useEffect patterns.

#### Requirements:

1. **User Profile Form:**
   - Edit user information
   - Avatar upload
   - Address management
   - Preferences settings
   - Real-time validation
   - Auto-save functionality

2. **Data Table with Filters:**
   - Search functionality with debouncing
   - Multiple filter options
   - Sorting
   - Pagination
   - Data fetching with useEffect
   - Cancel pending requests

3. **Real-time Notifications:**
   - WebSocket connection
   - Notification display
   - Mark as read functionality
   - Notification preferences
   - Cleanup on unmount

4. **Activity Log:**
   - Track user actions
   - Persist to localStorage
   - View recent activities
   - Clear log functionality
   - Export functionality

5. **Settings Panel:**
   - Theme toggle (dark/light)
   - Language selection
   - Notification preferences
   - Privacy settings
   - Persist settings to localStorage

#### Technical Requirements:

- Use controlled components for all forms
- Implement comprehensive validation
- Use useEffect for all side effects
- Proper cleanup for all effects
- Functional updates where appropriate
- Derived state instead of unnecessary useEffect
- TypeScript with strict mode
- Proper error handling
- Loading states for async operations

#### Bonus Features:
- Add unit tests for validation logic
- Implement undo/redo for form changes
- Add form field autocomplete
- Implement keyboard shortcuts
- Add analytics tracking

#### Evaluation Criteria:
- Code quality and organization
- Proper form handling
- Correct useEffect usage
- TypeScript type safety
- User experience
- Error handling
- Performance considerations
- Accessibility

This challenge will test your understanding of all concepts from this session and push you to apply them in a real-world scenario.

---

## Common Mistakes to Avoid

### 1. Not preventing default in forms
```tsx
// ❌ Wrong
const handleSubmit = (e) => {
  // Page will reload!
};

// ✅ Correct
const handleSubmit = (e) => {
  e.preventDefault();
};
```

### 2. Using value instead of checked for checkboxes
```tsx
// ❌ Wrong
<input type="checkbox" value={checked} />

// ✅ Correct
<input type="checkbox" checked={checked} />
```

### 3. Making useEffect function async
```tsx
// ❌ Wrong
useEffect(async () => {
  const data = await fetch('/api');
}, []);

// ✅ Correct
useEffect(() => {
  const fetchData = async () => {
    const data = await fetch('/api');
  };
  fetchData();
}, []);
```

### 4. Missing dependencies in useEffect
```tsx
// ❌ Wrong
useEffect(() => {
  console.log(count);
}, []);

// ✅ Correct
useEffect(() => {
  console.log(count);
}, [count]);
```

### 5. Not cleaning up intervals
```tsx
// ❌ Wrong
useEffect(() => {
  setInterval(() => {}, 1000);
}, []);

// ✅ Correct
useEffect(() => {
  const interval = setInterval(() => {}, 1000);
  return () => clearInterval(interval);
}, []);
```

### 6. Using useEffect for derived state
```tsx
// ❌ Wrong
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ Correct
const fullName = `${firstName} ${lastName}`;
```

### 7. Stale closures in setInterval
```tsx
// ❌ Wrong
useEffect(() => {
  setInterval(() => {
    console.log(count); // Stale!
  }, 1000);
}, []);

// ✅ Correct
useEffect(() => {
  setInterval(() => {
    setCount(prev => prev + 1); // Latest value
  }, 1000);
}, []);
```

### 8. Not handling loading states
```tsx
// ❌ Wrong
const handleSubmit = async () => {
  const data = await fetch('/api');
  setData(data);
};

// ✅ Correct
const handleSubmit = async () => {
  setLoading(true);
  try {
    const data = await fetch('/api');
    setData(data);
  } finally {
    setLoading(false);
  }
};
```

---

## Additional Resources

- [React Documentation - Forms](https://react.dev/learn/add-react-to-a-website#adding-interactivity)
- [React Documentation - useEffect](https://react.dev/reference/react/useEffect)
- [React Documentation - You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [React Hooks FAQ](https://react.dev/reference/react#hooks-faq)

---

**Congratulations on completing Session 4!** You now have a solid understanding of Forms and useEffect in React. Continue practicing with the exercises and challenge to reinforce these concepts before moving to the next session.
# ✅ Step 1: Set Prettier as Default Formatter

1. Open **VS Code**
2. Press **`Ctrl + Shift + P`**
3. Type **`Format Document With...`**
4. Select **`Configure Default Formatter`**
5. Choose **`Prettier - Code formatter`**

👉 This tells VS Code: *“Always use Prettier when formatting.”*

---

# ✅ Step 2: Enable Format on Save (IMPORTANT)

### Option A: Through Settings UI

1. Press **`Ctrl + ,`**
2. Search **`format on save`**
3. ✅ Check **Editor: Format On Save**

---

# ✅ Step 3: Make Sure Prettier Is Using `.prettierrc`

Prettier automatically detects `.prettierrc` **if it’s in project root**.

Your config is **100% valid** ✅

```json
{
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": false,
  "bracketSpacing": true,
  "arrowParens": "always"
}
```

---

# ✅ Step 4: Test It

1. Open any `.js`, `.ts`, `.jsx`, `.tsx`, `.json` file
2. Mess up formatting
3. Press **`Ctrl + S`**
4. 🎉 Code auto-formats

---

## ❌ Common Problems (Check These)

### 🔴 Not formatting?

* File must be **saved**
* File must be supported by Prettier
* Prettier extension must be **enabled**
* `.prettierrc` must be in **root folder**

---

# 🔍 Quick Debug Test

Run manually:

```
Shift + Alt + F
```

If this works → format on save will also work.

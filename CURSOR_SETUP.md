# ✅ Cursor Setup Complete!

## What I've Done:

1. ✅ Installed `ipykernel` in your virtual environment
2. ✅ Created a Jupyter kernel named "Python (Quant Finance .venv)"
3. ✅ Created `.vscode/settings.json` to configure Cursor for inline notebooks
4. ✅ Added `%matplotlib inline` to ensure plots display inline
5. ✅ Updated notebook metadata to use the correct kernel

## 🚀 How to Run in Cursor:

### Step 1: Open the Notebook
- The notebook `american_call_mc_pricing.ipynb` should already be open

### Step 2: Select the Kernel
1. Look at the **top-right corner** of the notebook
2. You should see a kernel selector button
3. Click it and select: **"Python (Quant Finance .venv)"**
   - OR select: **".venv/bin/python"** from the list

### Step 3: Run Cells
- **Run single cell**: `Shift + Enter`
- **Run all cells**: `Cmd+Shift+P` → type "Run All Cells"
- **Run cell and move to next**: `Shift + Enter`
- **Run cell and stay**: `Ctrl + Enter`

### Step 4: View Outputs
- Outputs will appear **directly below each cell** (not in browser!)
- Plots will render inline
- You can split the editor to see code and output side-by-side

## 🎯 If It Still Opens in Browser:

1. **Check kernel selection**: Make sure "Python (Quant Finance .venv)" is selected
2. **Reload Cursor**: `Cmd+Shift+P` → "Developer: Reload Window"
3. **Check settings**: The `.vscode/settings.json` should have:
   ```json
   "jupyter.jupyterServerType": "local"
   ```

## 📊 Expected Behavior:

- ✅ Notebook runs **inline in Cursor** (not browser)
- ✅ Outputs appear **below cells**
- ✅ Plots display **inline**
- ✅ You can **split tabs** to see code and output side-by-side
- ✅ Interactive widgets work inline

## 🔧 Troubleshooting:

**"No kernel selected"**
- Click kernel selector (top-right)
- Choose "Python (Quant Finance .venv)"

**"Module not found"**
- Make sure kernel is using `.venv`
- Run in terminal: `source .venv/bin/activate && pip install -r requirements.txt`

**"Plots not showing"**
- The `%matplotlib inline` is already added
- Restart kernel: `Cmd+Shift+P` → "Notebook: Restart Kernel"

---

**You're all set!** Just select the kernel and start running cells! 🎉


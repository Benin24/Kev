# Deploying to Azure App Service

## Option A — Azure Portal (GUI, easiest for screenshots)

1. Go to https://portal.azure.com and sign in.
2. Search **App Services** → **Create** → **Web App**.
3. Fill in:
   - **Resource Group**: create new, e.g. `portfolio-rg`
   - **Name**: unique, e.g. `kev-portfolio` (this becomes `kev-portfolio.azurewebsites.net`)
   - **Publish**: Code
   - **Runtime stack**: Node 20 LTS (or PHP/HTML5 if offered — Node works fine for static files too)
   - **Region**: closest to you (e.g. Central India)
   - **Pricing plan**: Free F1 (for a student demo)
4. Click **Review + create** → **Create**. Wait for deployment to finish.
5. Go to the resource → **Deployment Center**.
   - Choose **Local Git** or **GitHub** as the source.
   - If GitHub: push this folder to a repo first, then connect it.
   - If Local Git: Azure gives you a git remote URL — push your code with:
     ```
     git init
     git add .
     git commit -m "portfolio site"
     git remote add azure <the-url-azure-gives-you>
     git push azure main
     ```
6. Once deployed, open the app URL (`https://kev-portfolio.azurewebsites.net`) to confirm it's live — screenshot this for your report.

## Option B — Azure CLI (faster, good to show command-line evidence too)

```bash
az login
az group create --name portfolio-rg --location centralindia
az appservice plan create --name portfolio-plan --resource-group portfolio-rg --sku F1 --is-linux
az webapp create --resource-group portfolio-rg --plan portfolio-plan --name kev-portfolio --runtime "NODE:20-lts"
az webapp deployment source config-zip --resource-group portfolio-rg --name kev-portfolio --src portfolio-site.zip
```

To make the zip:
```bash
cd portfolio-site
zip -r ../portfolio-site.zip .
```

## Notes for your Q1 write-up

- Document each step with a screenshot: resource creation, deployment center, and the final live site.
- For the **comparison** part of Q1, note that Azure App Service (PaaS) abstracts the VM/runtime entirely, similar to Google App Engine — both auto-scale and manage patching, but Azure integrates more tightly with Active Directory/enterprise auth while App Engine integrates more tightly with GCP's data services.
- Cost: both offer free/low tiers for demos (Azure F1 free tier vs. App Engine's free daily quota).

# KubeVoIP Docs

Source for [docs.kubevoip.com](https://docs.kubevoip.com).

The API reference is generated from the platform repository:

```bash
python ../kubevoip/scripts/export-docs-reference.py --output content/docs/reference/api.md
npm install
npm run dev
npm run build
```

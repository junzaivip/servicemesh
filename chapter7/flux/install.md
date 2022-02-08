

brew install fluxcd/tap/flux

export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>

flux check --pre

flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=servicemesh \
  --branch=main \
  --path=github/flux \
  --personal
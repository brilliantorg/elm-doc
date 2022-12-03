# Updating the internal elm-docs site

## Setup

- Install poetry (see `https://python-poetry.org/docs/`)

- Install git lfs.  Without lfs git will not get large files (e.g. src/elm_doc/assets/assets.tar.gz)
    `brew install git-lfs`
    or
    `sudo apt install git-lfs`

- clone this repo and its submodule:
    ```
    git clone --recurse-submodules --remote-submodules git@github.com:brilliantorg/elm-doc.git
    ```

## Updating the elm_docs wheel

- If you've made changes to the docs themselves ([brilliantorg/package.elm-lang.org](https://github.com/brilliantorg/package.elm-lang.org/tree/master)), update the submodule:
  ```
  cd vendor/package.elm-lang.org
  git checkout master
  git pull
  cd ../..
  ```

- Update pyproject.toml:
  - bump the version (e.g. `1.0.2` -> `1.0.3`)

- Build the wheel file:
  ```
  poetry build
  ```

- Copy the wheel file into your brilliant tree, e.g
  ```
  cp dist/elm_doc-1.0.3-py3-none-any.whl ~/src/brilliant/local_wheels
  ```

- Remove the old wheel file:
  ```
  cd /src/brilliant/local_wheels
  git rm local_wheels/elm_doc-1.0.2-py3-none-any.whl
  ```

- Update brilliant requirements.txt to use the new wheel, e.g.
  ```
  - ./local_wheels/elm_doc-1.0.2-py3-none-any.whl
  ---
  + ./local_wheels/elm_doc-1.0.3-py3-none-any.whl
  ```
  note: see the [https://www.notion.so/brilliantorg/How-to-add-a-new-python-dependency-898d170699404ff68ebacf9afc48c18e](instructions for adding a new dependency) to make sure you don't accidentally cause any other dependency changes

## create a PR for both this repo and the brilliant repo and merge your changes

- for this repo, be sure to select the `brilliantorg/elm-doc` repo as your merge base rather than `ento/elm-doc`

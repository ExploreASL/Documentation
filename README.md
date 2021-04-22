## [Documentation](https://exploreasl.github.io/Documentation/)

This repository includes the interactive GitHub pages documentation of ExploreASL.

### Required libraries

- Documentation [mkdocs](https://www.mkdocs.org/)
- Documentation [mike](https://github.com/jimporter/mike)

### Build process

- Make sure that your local `gh-pages` branch is up-to-date and that you don't overwrite previous versions of the **ExploreASL** documentation. Reverting the following step is not possible.
- Update `gh-pages` using `mike deploy --push --update-aliases 1.x.x latest`.
- Update `default` using `mike set-default --push latest`.
- **No** need to run `mkdocs build` or other commands.
- Testing can be done using `mike serve`.


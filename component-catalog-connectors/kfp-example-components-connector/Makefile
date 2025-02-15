#
# Copyright 2021-2022 Elyra Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

.PHONY: help clean lint test-dependencies source-install install dist
.PHONY: publish test-publish test

SHELL:=/bin/bash

PACKAGE_NAME=elyra-examples-kfp-catalog

help:
# http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

clean:
	rm -rf dist/
	rm -rf build/
	rm -rf *.egg-info
	- pip uninstall -y $(PACKAGE_NAME)

test-dependencies:
	@pip install -q -r test_requirements.txt

lint: test-dependencies
	flake8 .

dist: clean lint  ## Build distribution
	python setup.py bdist_wheel sdist	

source-install: dist  ## Install pipeline components examples package from source 
	pip install .

install:  ## Install pipeline components examples package from PyPI
	pip install $(PACKAGE_NAME)

test:  source-install  ## Run automated tests
	pytest tests/

test-publish:  dist ## Upload package to test PyPI
	twine upload --repository testpypi dist/*

publish:  dist ## Upload package to PyPI
	twine upload dist/*
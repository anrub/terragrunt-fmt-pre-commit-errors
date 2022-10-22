### Describe the bug

The function [is_hook_run_on_whole_repo](https://github.com/antonbabenko/pre-commit-terraform/blob/master/hooks/_common.sh#L145) checks if the hook is executed on all files by verifying the passed in files (supplied from pre-commit itself) with the maximal checkable files, by running [git ls-files](https://github.com/antonbabenko/pre-commit-terraform/blob/master/hooks/_common.sh#L161), this is never true, if the files get partitioned by [xargs from pre-commit](https://github.com/pre-commit/pre-commit/blob/0239b27f4ffb37d56b94ac3f9aecef22300daf19/pre_commit/languages/helpers.py#L132), which happens if there are a lot of files in the repository.


### How can we reproduce it?

Run "terragrunt fmt" in a large repo and compare time to "pre-commit run -a"

e.g. test repository: https://github.com/anrub/terragrunt-fmt-pre-commit-errors 


```bash
$ time terragrunt hclfmt

real	0m0,936s
user	0m0,582s
sys	0m0,598s
```

```bash
$ time pre-commit run -a

real	0m7,992s
user	0m31,527s
sys	0m12,224s
```

### Environment information

* OS:  Ubuntu 22.04.1

* `uname -a` output:

```
Linux xx-xubuntu 5.15.0-48-generic #54-Ubuntu SMP Fri Aug 26 13:26:29 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

* Tools availability and versions:

[see test repository.
](https://github.com/anrub/terragrunt-fmt-pre-commit-errors/blob/main/.tool-versions)

* `.pre-commit-config.yaml`:

[see test repository](https://github.com/anrub/terragrunt-fmt-pre-commit-errors/blob/main/.pre-commit-config.yaml).

workspace(name = "test")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
COMMIT="d8e790c24d56a5ad01ddec74b03546b23d7e4e77"

http_archive(
    name = "io_tweag_rules_nixpkgs",
    strip_prefix = "rules_nixpkgs-"+COMMIT,
    urls = ["https://github.com/tweag/rules_nixpkgs/archive/"+COMMIT+".tar.gz"],
)

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_git_repository", "nixpkgs_package", "nixpkgs_local_repository")

#provide nixpkgs globally, you can ommit this, and rely on default channel
nixpkgs_local_repository(
    name = "nixpkgs",
    nix_file = "//nixpkgs:default.nix",
)

nixpkgs_package(
    name = "craftsman",
    attribute_path = "image",
    build_file = "//craftsman:BUILD",
    nix_file = "//craftsman:default.nix",
    repositories = {
      "nixpkgs": "//nixpkgs:default.nix", #overrides for imports within file
      "ndn": "//nixpkgs:nix-docker-nix.nix",
    },
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "29d109605e0d6f9c892584f07275b8c9260803bf0c6fcb7de2623b2bedc910bd",
    strip_prefix = "rules_docker-0.5.1",
    urls = ["https://github.com/bazelbuild/rules_docker/archive/v0.5.1.tar.gz"],
)


load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_load",
    container_repositories = "repositories",
)

container_repositories()

# load image generated by bazel-nix-mix to bazel workspace
container_load(
  name = "craftsman_base",
  file = "@craftsman//:image",
)
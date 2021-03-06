# Copyright 2019 Istio Authors. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
#

licenses(["notice"])

load(
    "@envoy//bazel:envoy_build_system.bzl",
    "envoy_cc_binary",
    "envoy_cc_library",
    "envoy_cc_test",
    "envoy_package",
)
load(
    "@com_github_google_flatbuffers//:build_defs.bzl",
    "DEFAULT_FLATC_ARGS",
    "flatbuffer_library_public",
)

envoy_package()

envoy_cc_library(
    name = "context",
    srcs = [
        "context.cc",
        "util.cc",
    ],
    hdrs = [
        "context.h",
        "istio_dimensions.h",
        "util.h",
    ],
    repository = "@envoy",
    visibility = ["//visibility:public"],
    deps = [
        ":node_info_fb_cc",
        "@envoy//source/extensions/common/wasm/null:null_plugin_lib",
    ],
)

envoy_cc_library(
    name = "proto_util",
    srcs = [
        "proto_util.cc",
    ],
    hdrs = [
        "proto_util.h",
    ],
    repository = "@envoy",
    visibility = ["//visibility:public"],
    deps = [
        ":node_info_fb_cc",
        "@com_google_protobuf//:protobuf",
        "@envoy//source/extensions/common/wasm/null:null_plugin_lib",
    ],
)

envoy_cc_library(
    name = "istio_dimensions",
    hdrs = [
        "istio_dimensions.h",
    ],
    repository = "@envoy",
    visibility = ["//visibility:public"],
)

envoy_cc_test(
    name = "proto_util_test",
    size = "small",
    srcs = ["proto_util_test.cc"],
    repository = "@envoy",
    deps = [
        ":node_info_fb_cc",
        ":proto_util",
        "@envoy//source/extensions/common/wasm:wasm_lib",
    ],
)

envoy_cc_test(
    name = "util_test",
    size = "small",
    srcs = ["util_test.cc"],
    repository = "@envoy",
    deps = [
        ":context",
        "@envoy//source/extensions/common/wasm:wasm_lib",
    ],
)

envoy_cc_test(
    name = "istio_dimensions_test",
    size = "small",
    srcs = ["istio_dimensions_test.cc"],
    external_deps = ["abseil_hash_testing"],
    repository = "@envoy",
    deps = [
        ":istio_dimensions",
    ],
)

envoy_cc_binary(
    name = "proto_util_speed_test",
    testonly = True,
    srcs = ["proto_util_speed_test.cc"],
    external_deps = [
        "benchmark",
    ],
    repository = "@envoy",
    deps = [
        ":node_info_fb_cc",
        ":proto_util",
        "@envoy//source/common/stream_info:filter_state_lib",
        "@envoy//source/extensions/common/wasm:wasm_interoperation_lib",
        "@envoy//source/extensions/common/wasm:wasm_lib",
    ],
)

flatbuffer_library_public(
    name = "node_info_fbs",
    srcs = ["node_info.fbs"],
    outs = [
        "node_info_bfbs_generated.h",
        "node_info_generated.h",
    ],
    flatc_args = DEFAULT_FLATC_ARGS + ["--bfbs-gen-embed"],
    language_flag = "-c",
)

cc_library(
    name = "node_info_fb_cc",
    srcs = [":node_info_fbs"],
    hdrs = [":node_info_fbs"],
    features = ["-parse_headers"],
    linkstatic = True,
    deps = ["@com_github_google_flatbuffers//:runtime_cc"],
)

genrule(
    name = "nlohmann_json_hpp",
    srcs = ["@com_github_nlohmann_json_single_header//file"],
    outs = ["nlohmann_json.hpp"],
    cmd = "cp $< $@",
)

envoy_cc_library(
    name = "json_util",
    srcs = ["json_util.cc"],
    hdrs = [
        "json_util.h",
        ":nlohmann_json_hpp",
    ],
    repository = "@envoy",
    visibility = ["//visibility:public"],
    deps = [
        "@envoy//source/extensions/common/wasm:wasm_lib",
    ],
)

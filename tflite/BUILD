# Copyright 2019 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("//:libedgetpu_cc_rules.bzl", "libedgetpu_cc_binary", "libedgetpu_cc_library")

package(default_visibility = ["//visibility:public"])

licenses(["notice"])

libedgetpu_cc_library(
    name = "custom_op_data",
    srcs = ["custom_op_data.cc"],
    hdrs = ["custom_op_data.h"],
    deps = [
        "//port",
        "@flatbuffers",
    ],
)

libedgetpu_cc_library(
    name = "custom_op",
    srcs = ["custom_op.cc"],
    hdrs = ["custom_op.h"],
    deps = [
        ":custom_op_data",
        "//api:layer_information",
        "//driver:package_registry",
        "//port",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
)

libedgetpu_cc_library(
    name = "custom_op_user_data_direct",
    srcs = ["custom_op_user_data_direct.cc"],
    hdrs = ["custom_op_user_data_direct.h"],
    deps = [
        ":custom_op",
        ":custom_op_data",
        "//api:driver",
        "//driver:package_registry",
        "//port",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_context_direct_header",
    hdrs = ["edgetpu_context_direct.h"],
    deps = [
        ":custom_op_data",
        ":custom_op_user_data_direct",
        "//api:driver",
        "//api:driver_factory",
        "//api:driver_options_fbs",
        "//api:package_reference",
        "//driver:driver_factory",
        "//port",
        "//port:std_mutex_lock",
        "//port:thread_annotations",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_manager_direct_header",
    hdrs = ["edgetpu_manager_direct.h"],
    deps = [
        ":custom_op_data",
        ":custom_op_user_data_direct",
        ":edgetpu_context_direct_header",
        "//api:driver",
        "//api:driver_factory",
        "//api:driver_options_fbs",
        "//api:package_reference",
        "//driver:driver_factory",
        "//port",
        "//port:std_mutex_lock",
        "//port:thread_annotations",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_context_direct",
    srcs = ["edgetpu_context_direct.cc"],
    hdrs = ["edgetpu_context_direct.h"],
    deps = [
        ":custom_op_data",
        ":custom_op_user_data_direct",
        ":edgetpu_manager_direct_header",
        "//api:driver",
        "//api:driver_factory",
        "//api:driver_options_fbs",
        "//api:package_reference",
        "//driver:driver_factory",
        "//port",
        "//port:std_mutex_lock",
        "//port:thread_annotations",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_manager_direct",
    srcs = ["edgetpu_manager_direct.cc"],
    hdrs = ["edgetpu_manager_direct.h"],
    deps = [
        ":custom_op_data",
        ":custom_op_user_data_direct",
        ":edgetpu_context_direct",
        "@com_google_absl//absl/strings:str_format",
        "//api:driver",
        "//api:driver_factory",
        "//api:driver_options_fbs",
        "//api:package_reference",
        "//api:runtime_version",
        "//driver:driver_factory",
        "//port",
        "//port:std_mutex_lock",
        "//port:thread_annotations",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
)

libedgetpu_cc_library(
    name = "custom_op_direct",
    srcs = ["custom_op_direct.cc"],
    deps = [
        ":custom_op",
        ":custom_op_data",
        ":custom_op_user_data_direct",
        ":edgetpu_c",  # buildcleaner: keep
        ":edgetpu_context_direct",
        ":edgetpu_delegate_for_custom_op_tflite_plugin",  # buildcleaner: keep
        ":edgetpu_manager_direct",
        "//api:driver",
        "//port",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:context",
    ],
    alwayslink = 1,
)

libedgetpu_cc_library(
    name = "tensor_data_controller",
    srcs = ["tensor_data_controller.cc"],
    hdrs = ["tensor_data_controller.h"],
    deps = [
        "//port",
        "@flatbuffers",
        "@org_tensorflow//tensorflow/lite:framework",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_context_factory",
    srcs = ["edgetpu_context_factory.cc"],
    hdrs = ["edgetpu_context_factory.h"],
    deps = [
        ":edgetpu_context_direct",
        "//port",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:framework",
    ],
)

# Set EXTRA_LOGGING to DUMMY to diable extra logging in text protobuf.
config_setting(
    name = "no_extra_logging",
    values = {"define": "EXTRA_LOGGING=DUMMY"},
)

libedgetpu_cc_library(
    name = "edgetpu_delegate_for_custom_op",
    srcs = [
        "edgetpu_delegate_for_custom_op.cc",
    ],
    hdrs = ["edgetpu_delegate_for_custom_op.h"],
    deps = [
        "//port",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:kernel_api",
        "@org_tensorflow//tensorflow/lite:util",
    ],
)

libedgetpu_cc_library(
    name = "edgetpu_delegate_for_custom_op_tflite_plugin",
    srcs = [
        "edgetpu_delegate_for_custom_op_tflite_plugin.cc",
    ],
    deps = [
        ":edgetpu_delegate_for_custom_op",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "//tflite/public:edgetpu",
        "@org_tensorflow//tensorflow/lite:kernel_api",
    ],
    alwayslink = 1,
)

libedgetpu_cc_library(
    name = "edgetpu_c",
    srcs = [
        "edgetpu_c.cc",
    ],
    deps = [
        ":edgetpu_delegate_for_custom_op",
        "//port",
        "//tflite/public:edgetpu",
        "//tflite/public:edgetpu_c",
    ],
    alwayslink = 1,
)

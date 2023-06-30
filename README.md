#!/bin/bash

# 定义日志文件路径
LOG_FILE="target/surefire-reports/test.log"

# 检查日志文件是否存在
if [ ! -f "$LOG_FILE" ]; then
  echo "日志文件不存在！"
  exit 1
fi

# 提取失败的测试用例和原因
failed_tests=$(grep -E "Failed tests:" "$LOG_FILE" | sed -n 's/.*Failed tests: \[\([^]]*\)\].*/\1/p')
failed_reasons=$(grep -A1 "Failed tests:" "$LOG_FILE" | grep -v "Failed tests:")

# 提取错误的测试用例和原因
error_tests=$(grep -E "Tests in error:" "$LOG_FILE" | sed -n 's/.*Tests in error: \[\([^]]*\)\].*/\1/p')
error_reasons=$(grep -A1 "Tests in error:" "$LOG_FILE" | grep -v "Tests in error:")

# 打印失败的测试用例及原因
if [ -n "$failed_tests" ]; then
  echo "失败的测试用例："
  echo "$failed_tests"
  echo "失败原因："
  echo "$failed_reasons"
else
  echo "没有失败的测试用例。"
fi

# 打印错误的测试用例及原因
if [ -n "$error_tests" ]; then
  echo "错误的测试用例："
  echo "$error_tests"
  echo "错误原因："
  echo "$error_reasons"
else
  echo "没有错误的测试用例。"
fi

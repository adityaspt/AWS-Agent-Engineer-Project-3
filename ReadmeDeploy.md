robocopy . ..\submit_copy\project /E /XD venv build agentcore node_modules __pycache__ .pytest_cache

Compress-Archive -Path ..\submit_copy\project -DestinationPath ..\Project3_submission.zip

Run these commands from
D:\PersonalProjects\UdacityFutureAgent\Project3\AWS-Agent-Engineer-Project-3\project
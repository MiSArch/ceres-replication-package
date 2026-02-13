# Ceres Replication package

This replication package allows you to recreate the scientific studies and experiments conducted during the development of Ceres as described in the accompanying paper.

It contains four parts:
1. The requirements engineering (RE) documents in the `Requirements_Engineering` directory, containing the interview questions and responses of the RE phase and the resulting list of requirements.
2. The architectural decision records (ADRs) in the `System_Design` directory, containing the list of ADRs which were decided upon during the system design of Ceres.
3. The usability study in the evaluation phase in the `Evaluation_User_Study` directory, containing the task definition for the usability evaluation of Ceres and the results, both as Excel and properly formatted in a PDF.
4. The results of the experiments made using Ceres in the `Evaluation_SLO_Experiments` directory, which contains the results of the different experiment runs and a guide how to recreate the experiment results for validation.
5. Some screenshots of the Frontend and the Dashboard which cannot be found in the paper are displayed in the `Screenshots` directory.
6. The actual source code of Ceres, without MiSArch, including a docker-compose and necessary config files is packed as a zip-file in the `ceres` directory.

# MiSArch

Please note that running MiSArch requires a significant amount of memory due to the database per service pattern and resulting amount of databases.
We recommend at leats 64GB RAM to conduct actual experiments.

## License
The entire replication package is licensed under the MIT license.

MIT License

Copyright (c) 2025 The authors of Ceres

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
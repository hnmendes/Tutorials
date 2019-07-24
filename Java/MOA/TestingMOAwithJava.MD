# How to running your first test code with MOA in eclipse.

First of all, download the MOA in https://sourceforge.net/projects/moa-datastream/, futhermore extract the following folders of the zip file: "lib" and "src" in some folder. 

Next,open your eclipse, then create a new java project.

Click with right mouse in your project and click in properties, next go to Java Build Path, then go to the "Libraries" tab, then click in the "Add External JARs..." button. After that, select all the files into the "lib" and "src".

![PrintScreen of the properties of the project](https://raw.githubusercontent.com/ricknunesmendes/Tutoriais/master/Java/MOA/Screenshot%20from%202019-07-23%2020-28-56.png)

Then create a simple package in your java project, after that, create a class called "Experiment".

As well, copy this code, and run your MOA code:

```
//package somepackage;
import moa.classifiers.trees.HoeffdingTree;
import moa.classifiers.Classifier;
import moa.core.TimingUtils;
import moa.streams.generators.RandomRBFGenerator;
import com.yahoo.labs.samoa.instances.Instance;
import java.io.IOException;


public class Experiment {

        public Experiment(){
        }

        public void run(int numInstances, boolean isTesting){
                Classifier learner = new HoeffdingTree();
                RandomRBFGenerator stream = new RandomRBFGenerator();
                stream.prepareForUse();

                learner.setModelContext(stream.getHeader());
                learner.prepareForUse();

                int numberSamplesCorrect = 0;
                int numberSamples = 0;
                boolean preciseCPUTiming = TimingUtils.enablePreciseTiming();
                long evaluateStartTime = TimingUtils.getNanoCPUTimeOfCurrentThread();
                while (stream.hasMoreInstances() && numberSamples < numInstances) {
                        Instance trainInst = stream.nextInstance().getData();
                        if (isTesting) {
                                if (learner.correctlyClassifies(trainInst)){
                                        numberSamplesCorrect++;
                                }
                        }
                        numberSamples++;
                        learner.trainOnInstance(trainInst);
                }
                double accuracy = 100.0 * (double) numberSamplesCorrect/ (double) numberSamples;
                double time = TimingUtils.nanoTimeToSeconds(TimingUtils.getNanoCPUTimeOfCurrentThread()- evaluateStartTime);
                System.out.println(numberSamples + " instances processed with " + accuracy + "% accuracy in "+time+" seconds.");
        }

        public static void main(String[] args) throws IOException {
                Experiment exp = new Experiment();
                exp.run(1000000, true);
        }
}

```

If the output is like that:

![Sucessfully screen](https://raw.githubusercontent.com/ricknunesmendes/Tutoriais/master/Java/MOA/Screenshot%20from%202019-07-23%2020-46-50.png)

#### You done!

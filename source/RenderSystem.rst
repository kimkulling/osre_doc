
.. _osre_render_system:

The Render System
=================

Multithreaded rendering - The idea
----------------------------------
In classic engines the rendering and the game logic will be done in the same thread - the main thread. So all the logic 
must share the time to get their tasks done like:
- Input handling
- User interaction
- Scene-updates
- And, last but to least - the rendering itself
So when you want to get a frame rate from 60 you have to work in a timeframe from 1/60 -> 1.67ms. Not too much.
To encouble this a little bit I the OSRE-Rendering will be done in a separate render-thread. Each update will be done 
one frame before.
This helps to get the render logic encapsulated from the rest and get more resources for a smooth render experience.
Of course new render-API's will be able to instrument multible threads for the rendering. To implement this logic 
a separate render-thread is an advantage as well. There is only one place where you have to look at.
All the rendering will be managed in a separate task. The main-thread can communicate with the back-end aka the render-thread
about the Render-system.

The Render-Graph
----------------
The rendering is managed by a render-graph:
- In each frame all the passes were iterated
- For each pass all the render-batches will be iterated
  - A batch iteration will set the uniform parameter
  - A batch iteration will set the material
  - A batch iteration will do all render calls.
  
It look like:
  
.. image:: images/OSRERenderGraph.svg
    

Render-Backend-Service
----------------------
We have a class called the RenderBackend-Service which is the fascade for the user to the render-backend. If you want to create a render 
window or you want to add a new mesh to the scene you have to do this via the RenderBackend-class:
::

    class RenderBackendService {
    public:
        RenderBackendService();
        virtual ~RenderBackendService();
        void setSettings(const Properties::Settings *config, bool moveOwnership);
        const Properties::Settings *getSettings() const;
        void sendEvent(const Common::Event *ev, const Common::EventData *eventData);
        PassData *getPassById(const c8 *id) const;
        PassData *beginPass(const c8 *id);
        RenderBatchData *beginRenderBatch(const c8 *id);
        void setMatrix(...);
        void setUniform(UniformVar *var);
        void setMatrixArray(const String &name, ui32 numMat, const glm::mat4 *matrixArray);
        void addMesh(Mesh *geo, ui32 numInstances);
        void addMesh(const CPPCore::TArray<Mesh *> &geoArray, ui32 numInstances);
        void updateMesh(Mesh *mesh);
        bool endRenderBatch();
        bool endPass();
        void clearPasses();
        void attachView();
        void resize(ui32 x, ui32 y, ui32 w, ui32 h);
        void syncRenderThread();
    };

::

To work with this you have to configure it and open the access to it:

::

       auto *rbService = new RenderBackendService();
       rbService->setSettings(mySettings, false);
       if (!m_rbService->open()) {
           // Error handling
       }
::

Render-API's
============
At this moment the following render-backends are implemented:

* OpenGL
* Vulkan (in progress)

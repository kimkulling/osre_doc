================================
Write your first Hello-World-App
================================

Prepare a workspace
===================
To get started you need to create a folder for your app andd add a CMakeLists.txt file into it:

::

   INCLUDE_DIRECTORIES(
     ${PROJECT_SOURCE_DIR}
     ../
   )

    SET ( 00_helloworld_src
      00_HelloWorld/HelloWorld.cpp
      00_HelloWorld/README.md
    )

    ADD_EXECUTABLE( HelloWorld
      ${00_helloworld_src}
    )

    link_directories(
      ${CMAKE_CURRENT_SOURCE_DIR}/../../ThirdParty/glew/Debug
      ${CMAKE_CURRENT_SOURCE_DIR}/../../ThirdParty/glew/Release
    )

    target_link_libraries ( HelloWorld   osre )

::

Your first Hello-World-Application
----------------------------------
Here the code:

::

    #include <osre/App/App.h>
    #include <osre/Common/Logger.h>
    #include <osre/RenderBackend/RenderBackendService.h>
    #include <osre/Scene/MeshBuilder.h>
    #include <osre/Scene/Camera.h>

    using namespace ::OSRE;
    using namespace ::OSRE::App;
    using namespace ::OSRE::RenderBackend;

    // To identify local log entries we will define this tag.
    static const c8 *Tag = "HelloWorldApp";

    ///
    /// The example application, will create the render environment and render a simple triangle onto it
    ///
    class HelloWorldApp : public AppBase {
        /// The transform block, contains the model-, view- and projection-matrix
        TransformMatrixBlock m_transformMatrix;
        /// The entity to render
        Entity *mEntity;
        /// The keyboard controller to rotate the triangle
        Scene::AnimationControllerBase *mKeyboardTransCtrl;

    public:
        /// The class constructor with the incoming arguments from the command line.
        HelloWorldApp(int argc, char *argv[]) :
                AppBase(argc, (const char **)argv),
                m_transformMatrix(),
                mEntity(nullptr),
                mKeyboardTransCtrl(nullptr) {
            // empty
        }

        /// The class destructor.
        ~HelloWorldApp() override {
            // empty
        }

    protected:
        bool onCreate() override {
            if (!AppBase::onCreate()) {
	        osre_error(Tag, "Error while creating application basics.");
                return false;
            }

            AppBase::setWindowsTitle("Hello-World sample! Rotate with keyboard: w, a, s, d, scroll with q, e");
            World *world = getActiveWorld();
            mEntity = new Entity("entity", *AppBase::getIdContainer(), world);
            Scene::Camera *camera = world->addCamera("camera_1");
            ui32 w, h;
            AppBase::getResolution(w, h);        
            camera->setProjectionParameters(60.f, (f32)w, (f32)h, 0.001f, 1000.f);

            Scene::MeshBuilder meshBuilder;
            RenderBackend::Mesh *mesh = meshBuilder.allocTriangles(VertexType::ColorVertex, BufferAccessType::ReadOnly).getMesh();
            if (nullptr != mesh) {
                mEntity->addStaticMesh(mesh);
                world->addEntity(mEntity);            
                camera->observeBoundingBox(mEntity->getAABB());
            }
            mKeyboardTransCtrl = AppBase::getTransformController(DefaultControllerType::KeyboardCtrl, m_transformMatrix);

            return true;
        }

        void onUpdate() override {
            RenderBackendService *rbSrv = getRenderBackendService();
            mKeyboardTransCtrl->update(rbSrv);

            rbSrv->beginPass(PipelinePass::getPassNameById(RenderPassId));
            rbSrv->beginRenderBatch("b1");

            rbSrv->setMatrix(MatrixType::Model, m_transformMatrix.m_model);

            rbSrv->endRenderBatch();
            rbSrv->endPass();

            AppBase::onUpdate();
        }
    };

    /// Helper function to generate the main function.
    OSRE_MAIN(HelloWorldApp)

::

Walkthrough
===========
Now let's take a deeper look what is going on in the code. We need to include some basic stuff for our first render-experiment.

Lets start with the headers:
----------------------------
::

    #include <osre/App/App.h>
    #include <osre/Common/Logger.h>
    #include <osre/RenderBackend/RenderBackendService.h>
    #include <osre/Scene/MeshBuilder.h>
    #include <osre/Scene/Camera.h>

::

To initialize the OSRE-rendersystem you can use the AppBase class. By including **App/App.h** the class and all dependencies will be included.

To make your application more verbose we want to log some messages. This is the reason to use the **Common/Logger.h** header. 

We want to render a single triangle. The **RenderBackend/RenderBackendService.h** will provide the API to add triangles. **Scene/MeshBuilder.h** offers
you a simple way to create triangle. So we need this include as well. 

And we want to look onto the scene. **Scene/Camera.h** provides an inferface for that.

Define your own application class
---------------------------------

::

    class HelloWorldApp : public AppBase {
        /// The transform block, contains the model-, view- and projection-matrix
        TransformMatrixBlock m_transformMatrix;
        /// The entity to render
        Entity *mEntity;
        /// The keyboard controller to rotate the triangle
        Scene::AnimationControllerBase *mKeyboardTransCtrl;

    public:
        /// The class constructor with the incoming arguments from the command line.
        HelloWorldApp(int argc, char *argv[]) :
                AppBase(argc, (const char **)argv),
                m_transformMatrix(),
                mEntity(nullptr),
                mKeyboardTransCtrl(nullptr) {
            // empty
        }

        /// The class destructor.
        ~HelloWorldApp() override {
            // empty
        }
    ...

::

We want to create a triangle. And we want to rotate this with our keyboard. So we need an attribute from the type **TransformMatrixBlock** . This class
provides a simple API to create a matrix with translation, scaling and rotating.

To manage the triangle in our scene we need an attribute from type **Entity**.

And last but not least we need an attribute to accss the keyboard input and control the animation of our triangle from the type **Scene::AnimationControllerBase**.
